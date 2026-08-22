# FastMetal-QAD：Mac 本地生成视频官方介绍

> 来源：黑粉科技（黑粉科技 · 资料整理 · 2026-08-20）
> 原文链接：https://mp.weixin.qq.com/s/L82C986ZT2TUspWLK8jQfw
> 整理日期：2026-08-22

---

## 一句话概括

FastMetal-QAD 是一套为 Apple Silicon 准备的开源视频生成模型与 MLX 运行时，不需要 CUDA，也不上云，DiT、采样器和解码器全部走 Mac 的 Metal GPU。

---

## 三档模型

一次发布三档：1.3B、5B、14B，来自不同的 Wan 基础模型，面向不同分辨率和内存档位。

| 模型 | 官方输出 | DiT 权重 | 目标内存 |
|------|----------|----------|----------|
| 1.3B | 480p, ~5秒 | 1.4GB | 16GB+ |
| 5B | 480p/720p, ~5秒 | 4.9GB | 16GB+ |
| 14B | 480p/720p, ~5秒 | 14GB | 36GB+ |

> ⚠️ DiT 大小 ≠ 完整仓库下载体积。Hugging Face 三个仓库已用存储约 13.4GB / 19.5GB / 42.3GB（含文本编码器、分词器、VAE、配置文件等）。

三档均为三步学生模型，DMD2 蒸馏 + 量化感知训练，affine INT8 在训练中量化而非训练后硬压。

---

## 官方速度（M4 Max 36GB）

| 模型 | 基线总耗时 | Fast 模式 | 基线峰值显存 |
|------|-----------|-----------|-------------|
| 1.3B · 480p | 110.14s | 45.19s | 3.87 GiB |
| 5B · 720p | 151.42s | 47.24s | 9.34 GiB |
| 14B · 480p | 601.82s | 211.14s | 21.68 GiB |

> ⚠️ **缓存条件关键**：基线含冷启动 umT5 编码成本（1.3B/14B ~18s，5B ~47s），Fast 行复用已缓存的提示词嵌入。151s vs 47s 不能直接理解为"开开关快三倍"。

社交平台展示的"30秒生成5秒视频""10.3秒"等数字为 warm prompt + Fast + Spatial fast 额外加速配置，非冷启动基线。

### MacBook Air M5 24GB 数据

- 1.3B：基线 156.2s / Fast 58.2s
- 5B：基线 200.1s / Fast 90.7s
- 14B：放不下 81 帧完整任务，仍归入 36GB+ 档位

---

## 显存优化设计

- umT5 文本编码器先以 bf16 加载，编码完成即释放，再加载 DiT
- 默认解码器用体积更小的 TAEHV（MIT 许可证），而非完整 Wan VAE
- MLX dense attention + 量化矩阵乘法跑在 Metal GPU
- DiT 矩阵权重 group size 64 affine INT8，归一化层和调制表保留 fp16
- 仓库直接提供预量化 MLX checkpoint
- 复用同一提示词命中内容寻址缓存

---

## 模式说明

| 模式 | 做了什么 | 适合 |
|------|----------|------|
| Fast | 少生成部分帧 + RIFE 插帧 | 快速预览 |
| Refine | 低分辨率生成 → 高分辨率去噪 | 补细节 |
| Quality | 改用完整 Wan VAE 解码 | 最终输出 |
| Prompt enhancement | 本地扩写短提示词并缓存 | 提示词不够完整 |
| Spatial fast | 低分辨率去噪后放大潜变量 | 实验性提速 |
| Draft attention | 窗口注意力加 sinks | 实验性探索 |

> Spatial fast 与 Draft attention 仍标为 experimental。

---

## 安装步骤

1. 环境：macOS 14+、Python 3.12.4、FFmpeg
2. 推荐用 uv 建虚拟环境
3. `brew install ffmpeg`
4. `uv venv --python 3.12 --seed`
5. `uv pip install "fastvideo[mlx]"`
6. 下载模型：`hf download FastVideo/FastMetal-1.3B-QAD --local-dir ./FastMetal-1.3B-QAD`

> 5B 来自 Wan2.2 TI2V，潜变量几何和时间步条件不同，需使用 5B 专用入口，不能照抄 1.3B 命令换目录。

---

## 选型指南

- **16GB**：从 1.3B 开始
- **24GB**：1.3B 和 5B 均有官方数据
- **36GB+**：14B 的目标档位
- 硬盘按完整仓库准备，不要只看 DiT 那一列

---

## 官方入口

- 官方发布：https://haoailab.com/blogs/fastmetal/
- FastVideo：https://github.com/hao-ai-lab/FastVideo
- Apple Silicon 安装：https://haoailab.com/FastVideo/getting_started/installation/mps/
- 模型合集：https://huggingface.co/collections/FastVideo/fastmetal

---

> ⚠️ 本文为官方资料中文整理，非本机实测。
