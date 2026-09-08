# 开源34天，MiniMax H3 被全世界"魔改"成了什么样？

**来源：** 微信公众号文章，2026-09-07
**链接：** https://mp.weixin.qq.com/s/Dwvt9G8c5gkkinzYiT_5vg
**相关：** [Codex + MiniMax H3 / Seedance 2.5 视频工作流](2026-09-07-codex-minimax-h3-seedance-video-workflow.md)（侧重 Codex 使用工作流，本文侧重 H3 开源生态全景）
**标签：** #MiniMaxH3 #视频生成 #开源生态 #ComfyUI #量化 #蒸馏

---

## 开篇：为什么这次开源不一样

2026-07-31 WAIC 发布，8-03 权重上线 HF。三天后社区开始爆发。

**H3 的三个独特之处：**
1. 第一个登顶 Artificial Analysis 视频榜单的开放权重模型（视频编辑第1、文生视频第2、图生视频第3）
2. 开源视频模型里唯一原生带同步立体声且对普通话支持良好
3. Day-0 生态密度前所未有：ComfyUI 原生支持、SGLang/vLLM/diffusers/DiffSynth 四大框架接入、NVIDIA/AMD 官方优化报告、华为昇腾等 16 家芯片厂商同日适配

---

## H3 完整系统三模块

| 模块 | 作用 | 状态 |
|------|------|------|
| H3-Base | 生成 768p 音视频 | ✅ 开放权重（CFG 蒸馏版）|
| H3-Context-IR | 多模态指令翻译 | 仅 API |
| H3-Regenerate-2K | 768p→2K 重生成 | 仅 API |

本地能跑到 768p，2K 要走 API。

---

## 挑战一：让它变快

### 厂商级优化

**NVIDIA Sol Engine**
- 8×GB200 比 Diffusers 快 3.95×
- RTX 5090 上 4.52×（消费级）
- H3 Super Acceleration：5 秒 1344×768 只要 6.85 秒，比 SGLang 基线快 22.2×

**fal H3 Max**
- 自研 RL 框架后训练，5 秒 768p 视频不到 3 秒生成
- 比官方端点吞吐高约 35×，比同质量模型平均快 15×
- 质量/提示理解/美学三项第 1（fal 内部评测）
- 限制：只到 768p，权重未公开

**LMSYS/SGLang 速度换质量定量表**
- 无损 1.95×
- 叠加 Cache-DiT 跳步 + SubBlock 稀疏可达 6.24×（SSIM 0.76–0.91）

**vLLM-Omni**
- 8×B300 上 10 秒 MP4 只要 8.7 秒生成
- 叠 FastH3 四步蒸馏

### 蒸馏级：20 步变 4 步

**LightX2V / ModelTC Turbo**（Apache-2.0）
- v1.0：8 步 + 768p 4 步
- Turbo-SLA：4 步蒸馏 + 85% 稀疏线性注意力合进一个 LoRA，5090 上约 2.5×
- Ref2VA：8 步 768p

**FastH3**（UCSD Hao AI Lab + Nuva Lab + NVIDIA FastGen）
- 4 步 DMD2 + 90% 稀疏注意力，Data-Free（只用提示词）
- 单 B200：15 秒从 678.7 秒→47.2 秒（14.38×）
- 8×B200：12.88 秒——比视频本身还短
- 后续下放到 DGX Spark（7–8×）和 Apple Silicon（需 36GB+）

**社区共识：4 步音频和快速运动会退化，6–8 步是甜点**

---

## 挑战二：塞进你的显卡

### ComfyUI 奠基两招

1. **AdaLN 剪枝**：约 40% 参数用查找表替代，体积缩小 40%，无质量损失
2. **INT8 ConvRot 量化 + 自定义 kernel**：总内存 123.6GB→42.5GB，RTX 3060 可跑

### 量化狂潮：一周铺满所有精度

| 精度 | 最低体积 | 代表 |
|------|---------|------|
| bf16 | 61.7GB | 官方 |
| pruned int8 | 19.5GB | ComfyUI 官方 |
| GGUF Q2 | ~3.78GB | MarxistLeninist IQ1_S |
| NVFP4/INT4 | ~10GB | DmitryDB/Abiray |
| MLX 4bit | — | Apple Silicon |

**核心发现：VRAM 是幌子，真正瓶颈是主机内存**
- tonyd2wild 用 3090 + 31GB 内存跑完 15 秒（关键参数：--disable-pinned-memory）
- 5090 上 36 个视频基准：无论 checkpoint 多大，峰值显存都是 31.7GB

### 不止 NVIDIA：全线开花

**Redis 作者 antirez 的 h3.c**
- 纯 C + Metal，不依赖 Python/PyTorch，MIT 许可
- M5 Max 上 512² 4 步约 3.5 秒
- "Mac 本地不推荐 H3"被一个人的项目改写

**AMD 消费级**
- Strix Halo（Radeon 8060S）1344×768 5 秒约 57 分钟
- RX 7800 XT 16GB 也跑通了

**华为昇腾**
- MindIE-SD 融合算子 + RainFusion 注意力
- Ascend-SACT 补丁让 Atlas 800I A2（8×910B3）稳定跑 15 秒 768p

---

## 挑战三：让它更强

### 后训练与全量微调

- **fal H3 Max**：唯一有规模的后训练版本
- **TenStrip 10Eros-Max**：唯一有规模的社区全量微调（改动集中在 0–31 层 QKV）

### 功能 LoRA（不到一个月约 20 个）

| LoRA | 功能 |
|------|------|
| Camera Motion | 推/拉/摇/移/环绕/手持 + 提示词库 |
| Wushu Action V7924 | 192 段武打片段、专业招式术语打标 |
| Spatial Physics | CLEVRER/WISA/PhyCo-Kubric 物理数据集，碰撞/堆叠/掉落/遮挡 |
| Looping Sketch Anime | 手绘循环动画 |
| Realism People | fal 16 组配置、1300+ 生成人工评审选出 |
| Turnaround | 三视图 |
| Prompt Rewriter | Qwen3.6-27B 微调，短提示→三段式 |

### 不改权重的三个巧思

1. **Semantic Bridge**（9-05）：跨架构蒸馏，SenseNova U1.5 做语义教师，11MB 独立适配器
2. **Ref Patchdiff**：112 个共享 key 存成 148MB 补丁，轻量 FL2VA 获得 Ref2VA 能力
3. **高频补丁**：t8star 给视频输入投影加 2×2 高频 patch，去"油腻感"

---

## 挑战四：突破 768p

四条路并行：

1. **直接原生出 1080p**：1920×1088 可直接出，但效果接近 720p，甜点 960×540 或 1344×768
2. **两阶段潜空间放大**（社区主流）：480p 草稿→潜空间放大 1.4–1.5×→低噪声二次采样，5090 上能出接近 1080p 的 15 秒以上片段
3. **外部超分到 4K**：SeedVR2 视频超分节点配合 H3 latent 适配器
4. **托管 API**：MiniMax 官方 Regenerate-2K；fal 提供 2K（$0.13/秒）与 4K（$0.16/秒）

---

## 脑洞区精选

**腾讯 world model**：训练 H3 做物理模拟器（弹珠台），让视频生成成为物理世界的可微表示

**无限直播**：循环工作流让 H3 连续生成不中断

**图像模型**：把 H3 当静态图生成器用

**三视图生成**：Turnaround LoRA 实现角色三视图一致性

**真人换纸片人**：风格迁移 LoRA 实现动漫风格化

---

## 关键链接汇总

| 资源 | 链接 |
|------|------|
| Hugging Face 模型卡 | https://huggingface.co/MiniMaxAI/MiniMax-H3 |
| GitHub 官方仓库 | https://github.com/MiniMax-AI/MiniMax-H3 |
| 技术博客 | https://www.minimax.io/blog/minimax-h3 |
| 部署门户 | https://design.minimax.io/h3 |
| 社区生态索引 | https://github.com/MiniMax-AI/awesome-minimax-h3-integration |
| SGLang H3 Cookbook | https://docs.sglang.io/cookbook/diffusion/MiniMax/MiniMax-H3 |
| ComfyUI Wiki | https://comfyui-wiki.com/en/news/2026-08-03-minimax-h3-open-weights-comfyui |

---

## 核心洞察

H3 开源 34 天内，社区从"让它变快"到"塞进你的显卡"到"让它更强"到"突破 768p"，四个挑战并行推进。最出圈的不是大厂优化，而是 Redis 作者 antirez 一个人用纯 C+Metal 在 Mac 上跑通了 H3——证明开源生态的爆发性远超预期。
