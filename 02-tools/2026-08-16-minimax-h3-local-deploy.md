# MiniMax H3 (H3-Base) 本地部署保姆级完整教程

> 来源：[MiniMax H3（H3-Base）本地部署保姆级完整教程](https://mp.weixin.qq.com/s/kKDnzPo_czjsgcE1NPdZDA) · 2026-08-16

---

## 概览

MiniMax H3 开源版本仅包含 **H3-Base 基座模型**，可本地部署输出 **768P 原生音视频**。
- H3-Context-IR（精准指令模块）和 H3-Regenerate-2K（超分模块）无开源权重
- 想要 2K 分辨率成片：需 **本地 H3-Base 生成 768P 基础片 + MiniMax 官方 API 做 2K 超分**

---

## 硬件门槛（必看，避免 OOM）

| 显存 | 可用模型版本 | 生成效果 | 适用人群 |
|------|--------------|----------|----------|
| 8GB | INT8 量化精简版 | 768P 短片段（3–5s），画质小幅压缩 | 入门体验、测试 |
| 16GB | INT8/FP8 量化版 | 768P 常规短视频（5–10s） | 主流创作者、自媒体 |
| 24GB+ (4090/A10) | BF16 原生完整版 | 满画质 768P、长镜头、角色锁定 | 工作室、重度创作 |
| 4×80G A100 | BF16 多卡并行 | 批量推理、企业私有化服务 | 算法团队、云端服务 |

**配套硬性要求**：
- 系统内存：最低 32GB，64GB 最优（模型加载占大量内存）
- 硬盘：NVMe 高速固态，模型完整包约 130GB，量化版 60–80GB，**禁止机械硬盘**
- 系统：Windows 10/11（CUDA 12.1+）、Ubuntu 22.04；AMD 卡支持 ROCm 但兼容性弱于 N 卡

---

## 模型两大分支（按需下载）

| 分支 | 用途 | 场景 |
|------|------|------|
| **FL2VA** | 文生视频、首尾帧图生视频 | 日常短视频、剧情生成首选 |
| **Ref2VA** | 参考视频动作迁移、人物形象锁定、镜头复刻 | 数字人、二创专用 |

**精度选择**：
- BF16 原版：画质拉满，显存占用最高
- INT8 量化：显存减半，画质损耗极小，**个人电脑首选**
- FP8 量化：显存更低，适合低配显卡极限运行

---

## 方案一：ComfyUI 可视化部署（新手零代码，推荐）

### 步骤 1：准备 ComfyUI 环境
- 下载整合版 ComfyUI 一键启动包，解压到**纯英文路径**
- 运行 `run_nvidia_gpu.bat` 启动，访问 `http://127.0.0.1:8188` 确认正常
- **更新 ComfyUI 到 0.30.0+**，旧版本无 H3 原生节点支持

### 步骤 2：下载模型权重

**渠道 1：HuggingFace 命令行（完整原版）**
```bash
pip install -U huggingface_hub
# 只下载 FL2VA（日常够用）
hf download MiniMaxAI/MiniMax-H3 --include "FL2VA/*" "model_index.json" --local-dir MiniMax-H3-FL2VA
# 需 Ref2VA 参考模型执行这条
hf download MiniMaxAI/MiniMax-H3 --include "Ref2VA/*" "model_index.json" --local-dir MiniMax-H3-Ref2VA
```

**渠道 2：社区量化精简包（低配显卡首选）**
直接下载适配 ComfyUI 的量化权重：
- `minimax_h3_fl2va_pruned_int8_convrot.safetensors`
- `minimax_h3_ref2va_pruned_int8_convrot.safetensors`

### 步骤 3：模型文件夹归类放置
```
ComfyUI/
├─ models/
│  ├─ diffusion_models/      # 主模型权重
│  ├─ text_encoders/         # Qwen3-VL 文本编码器
│  ├─ vae/                   # video_vae、audio_vae 解码器
```

**懒人技巧**：新建 `extra_model_paths.yaml` 填写外置路径，不用复制大文件到 ComfyUI 目录，节省磁盘空间。

### 步骤 4：加载官方工作流直接出片
- ComfyUI 顶部「Template Library」→「Video」选择官方工作流
- 节点自动绑定 H3 模型，设置 1280×768（16:9）、4–10 秒
- 提示词示例：
  - 正面：城市雨夜街道，汽车驶过灯光拖影，电影运镜，原生立体声环境雨声，画面流畅，人物细节清晰
  - 负面：画面撕裂、口型错位、模糊、水印、画面闪烁、音画不同步
- 成品保存在 `ComfyUI/output`，自带画面 + 合成立体声 MP4

---

## 方案二：SGLang 命令行部署（服务器批量推理、API 服务）

面向企业、算法开发者，搭建后端推理接口，支持批量生成、多卡并行。

```bash
# 环境依赖
pip install sglang torch torchvision torchaudio transformers accelerate

# FL2VA 文生视频推理服务
sglang serve --model-path MiniMaxAI/MiniMax-H3 --model-variant fl2va --host 0.0.0.0 --port 30010 --performance-mode speed

# Ref2VA 参考视频服务
sglang serve --model-path MiniMaxAI/MiniMax-H3 --model-variant ref2va --host 0.0.0.0 --port 30011
```

启动后通过 HTTP 接口调用生成视频，可对接自有剪辑平台、数字人系统。

---

## 实现 2K 高清成片完整工作流（本地 + API 组合）

```
本地 ComfyUI 用 H3-Base 生成 768P 基础音视频（画面、口型、音效全部定型）
        ↓
MiniMax 开放平台注册获取 API Key
        ↓
调用 H3-Regenerate-2K 超分 API，上传本地成片无损放大至 2K 分辨率
```

**优势**：保留本地创作素材隐私，同时拿到官方顶级超分画质，适合商业宣传片、广告视频。

---

## 显存优化技巧（低配显卡必用）

| 技巧 | 效果 |
|------|------|
| 启用模型量化（INT8/FP8） | 显存直接降低 40%–50% |
| CPU 内存卸载 | ComfyUI 开启 `model_cpu_offload`，闲置权重放入内存 |
| 缩短生成时长 | 优先生成 5 秒以内短视频，避免超长视频爆显存 |
| 分辨率锁定 768P | 不要强行拉高分辨率推理 |
| Turbo 加速 LoRA | 社区加速 LoRA 将生成步数从 12 步压缩至 4 步，速度提升 40% |

---

## 高频报错 & 解决方法

| 报错 | 解决方案 |
|------|----------|
| CUDA out of memory | 换 INT8 量化、缩短时长、开启 CPU 卸载、关闭其他占用程序 |
| ComfyUI 无 H3 节点 | 升级 ComfyUI 到最新版、检查模型路径、重启程序 |
| 音画不同步 | 用官方原版工作流，不要自定义修改 VAE 音频解码节点（H3 音视频联合生成） |
| 模型下载中断 | 魔搭 ModelScope 国内镜像下载，断点续传更稳定 |

---

## 商用授权补充

| 场景 | 说明 |
|------|------|
| 个人非商用 | H3-Base 开源模型完全免费使用、二次微调 |
| 企业商用 | 主体年收入超 2000 万美金，需向 MiniMax 申请商业授权 |
| 私有化部署企业 | 仅本地 H3-Base 无授权费用，调用 2K 超分 API 按平台按量计费 |

---

## 核心要点总结

1. **仅 H3-Base 开源**，本地跑 768P；2K 需配合官方 API
2. **个人首选 INT8 量化版**，显存减半、画质损耗极小
3. **新手走 ComfyUI 可视化**，有官方模板零代码出片
4. **企业批量走 SGLang**，搭建 API 服务对接自有系统
5. **商用注意授权门槛**，2000 万美金年收入线