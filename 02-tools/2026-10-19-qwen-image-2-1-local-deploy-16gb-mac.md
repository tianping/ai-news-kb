# Qwen-Image-2.1 本地部署实测：16GB MacBook Air 跑通（GGUF / MLX / Turbo 三条路线）

- **日期**：2026-10-19（收藏）
- **来源**：微信公众号「AI公园」，译自 Medium @hyperai
- **原标题**：Qwen Image 2.1 无审查版上手：16GB 的 Mac 就能跑
- **分类**：02-tools（本地部署 / 图像生成）
- **URL**：https://mp.weixin.qq.com/s/I2Y7fjkrGdvx4GxpeTa1OQ
- **英文原文**：https://medium.com/@hyperai/qwen-image-2-1-uncensored-local-deployment-a-16gb-macbook-air-can-run-it-3a98394f7bf4
- **相关笔记**：[Qwen-Image-2.1-Uncensored-GGUF：社区魔改本地生图版](../01-models/2026-10-17-qwen-image-2-1-uncensored-gguf.md)（模型介绍篇）

## 概述

Qwen-Image-2.1 是阿里 Qwen 团队的 **7B 扩散 Transformer**：原生 2K 输出，能文生图，也能用最多 10 张参考图改图，作者称中文字渲染在开源权重模型里最好。社区无审查版（abenzerps/Qwen-Image-2.1-Uncensored-GGUF）去掉了安全检查器。作者在 **M4 MacBook Air（16GB 统一内存）** 上用 MLX 路线完整跑通，并给出真实内存和速度数据。

## 为什么本地跑

- **隐私**：云端图像 API 会记录提示词；客户项目、未发布产品留在别人服务器上有风险。
- **成本**：苹果芯片出一张 1024² 的电费可以忽略，批量出图不额外花钱。
- **可复现**：固定种子输出确定，不会遇到 API 中途改口径。

⚠️ **许可证变了**：2.1 从 Apache 2.0 改成 **Qwen 研究许可证**，只允许研究和评估。要商用，可以回退到仍是 Apache 2.0 的 **Qwen-Image-2512**，或者找阿里谈商业授权。

## 三条部署路线

### 1. GGUF + ComfyUI（跨平台）
- 装 **ComfyUI-GGUF** 插件；文件分别放进 `models/diffusion_models`、`models/text_encoders`、`models/vae`。
- 节点：`Unet Loader (GGUF)`；`CLIPLoader` 类型选 `qwen_image`；`VAELoader` 照常。Comfy-Org 官方模板换掉加载节点就能用。
- **省显存**：扩散模型留在显存，文本编码器卸到内存（每条提示词只编码一次），能省 9–17GB 显存，速度基本不受影响。16GB 显存的卡撞到上限时，启动加 `--lowvram`。

### 2. MLX（苹果芯片原生）
- 权重：**JoyFusionAI/Qwen-Image-2.1-MLX-4bit**（4-bit 量化）。
- 需要 **MFLUX ≥ 0.20.0**（commit 8c00dab2 之后才支持 2.1），安装：`uv tool install --upgrade mflux`。

```bash
mflux-generate-qwen-2.1 \
  --model JoyFusionAI/Qwen-Image-2.1-MLX-4bit \
  --base-model qwen-image-2.1 \
  --prompt "A red fox in a snowy forest holding a wooden sign that says 'Hello'" \
  --steps 40 --low-ram
```

- `--low-ram`：提示词编码完就释放文本编码器，VAE 解码分块跑。在 M1 Max 上出 768²，峰值内存从 33–36GB 降到约 7GB。**16GB 的机器必须加这个参数。**

### 3. Turbo 蒸馏版（求快）
| 模型 | 步数 | 说明 |
|---|---|---|
| Viggle/Qwen-Image-2.1-viggle-turbo | 4 | 分布匹配蒸馏；有完整 14.2GB 版和 340MB LoRA；CFG 必须锁 1.0，要配自定义调度器；多参考图合成、换脸、人物一致性编辑效果不如基线（v0.1 预览版，官方文档也承认） |
| Tongyi-MAI/Z-Image-Turbo | 8 | 阿里通义出品，6B 参数，**Apache 2.0**（可商用），16GB 显存消费卡能跑；按 Layer 的 Elo 排名，盲测追平或超过 FLUX.2 Dev |

## 16GB MacBook Air 实测

- **环境**：M4 MacBook Air，10 核 CPU，16GB，macOS 26.6.2；MFLUX 0.20.0，Python 3.13.15。
- **磁盘**：权重约 16GB；带 Git LFS 缓存的完整仓库占 38GB，实际工作体积接近 20GB。
- **测试 1**：512×512、4 步，顺利出图，确认部署打通。
- **测试 2**：1024×1024、20 步，中文提示词（橘猫坐在中式木窗边，窗外是雨中的江南水乡……）。
  - **MLX 峰值内存 18.33GB**，超过物理内存。这是 MLX 分配过的峰值，不是 GPU 独占内存：统一内存让 GPU 能寻址整个内存池，吃紧时 macOS 把压缩后的页换到 SSD。代价是换页拖慢速度。
  - **每步 44.95 秒**，20 步约 15 分钟，适合排队批量跑，不适合交互。
- **出图质量**：整体构图连贯，毛发、木纹、水面倒影有摄影感，浅景深也按提示词执行。短板是毛发纹理局部重复、背景建筑经不起放大、远处招牌文字读不出（4-bit 本地模型意料之中）。适合文章配图、概念草图、创意初稿；商用排版图、产品级静物图要先测。
- **未测**：多参考图编辑。社区反馈 RTX 4060 Ti 16GB 用 INT8 约 15GB 显存能跑，但一张要几分钟。

## 结论

- 16GB Mac 证明「能跑」，不代表「跑得爽」。
- 本地适合打样、守隐私、省按张计费；批量几百张、原生 2K、Qwen-Image-Edit 这类更大的模型，交给按分钟计费的云 GPU 更划算。作者的说法是「本机打样，云端放量」。

## 备注

- 原文有几张表（GGUF 文件清单、步数/速度对照、选型建议）是图片，抓取时没拿到文字，相关数字以正文为准。
- 无审查模型没有安全过滤，使用时需自行遵守法律与平台规定；下载第三方权重优先选 GGUF/safetensors。
- Elo 排名、显存节省幅度等数字为原文转述，未核实。
