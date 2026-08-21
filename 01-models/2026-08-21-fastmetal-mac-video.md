# FastMetal：Mac 本地30秒生成视频，3.9 GiB 内存，无需云端显卡

> 来源：[Mac 本地30秒生成视频，不用云端，不用显卡](https://mp.weixin.qq.com/s/aBdcUBENG3AfsLf-doEriw) · AI快讯 · 2026-08-21

## 概述

UCSD Hao AI Lab 发布 **FastMetal**，在 Apple Silicon 上原生跑视频模型，无需 CUDA，无需云端，3.9 GiB 内存即可出片。

## 核心数据

| 指标 | 数值 |
|------|------|
| 内存占用 | 3.9 GiB |
| 出片速度 | 30秒（5秒480P视频） |
| 硬件要求 | Apple Silicon Mac（统一内存架构） |
| 推理框架 | MLX + Metal 后端 |
| 量化方式 | INT8 默认 |

## 三档模型

| 档位 | 参数量 | 分辨率 |
|------|--------|--------|
| 入门 | 1.3B | 480P |
| 标准 | 5B | 720P |
| 画质 | 14B | 高分辨率（需 36GB Mac Studio） |

## 技术原理：为什么 Mac 能跑？

传统认知：视频生成 = 云端 + 显卡 + 贵

FastMetal 打破了这个等式，关键不在算力，而在 **token 数量**：

- 视频去噪过程中 **70% 计算量是自注意力操作**
- 自注意力复杂度 O(tokens²)，瓶颈在 token 数量而非算力
- Apple Silicon **统一内存架构**：CPU 和 GPU 共享物理内存，无显存拷贝开销 → 消除了"显存墙"

## 加速策略

团队设计了四种加速模式，沿不同维度削减 token，可组合使用：
1. 丢帧重建
2. 降分辨率去噪
3. 二次精修
4. （其他组合模式）

## 对比：社区方案 vs FastMetal

| | MiniMax-H3 MLX 移植 | FastMetal |
|---|---|---|
| 内存 | 48 GB | 3.9 GB |
| 速度 | 30 分钟 | 30 秒 |
| 优化 | 仅移植 | 针对性注意力优化 |

速度差距约 **60 倍**，内存仅十分之一。

## 背景

2026 年多家大模型 API 大幅涨价，本地部署需求被推高。FastMetal 恰好踩在这一趋势上——内容创作脱离服务器限制，成本门槛和隐私门槛同时降低。

## 资源链接

| | 链接 |
|---|------|
| 博客 | https://haoailab.com/blogs/fastmetal |
| 代码 | https://github.com/hao-ai-lab/FastVideo |
| 模型 | https://huggingface.co/collections/FastVideo/fastmetal |

团队：UCSD Hao AI Lab，使命是"民主化大模型"，训练算力由 NVIDIA 提供。
