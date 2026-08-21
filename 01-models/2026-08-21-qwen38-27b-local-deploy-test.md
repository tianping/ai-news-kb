# Qwen3.8-27B 本地部署实测：无显卡也能跑，22 TPS 开启无限 token

> 来源：[免费，无限token时代，就这么来了？？（附完整教程）](https://mp.weixin.qq.com/s/CMDayD_eI6U0ZDVzwVmbqw) · 探索AGI · 2026-08-21

## 概述

无显卡情况下，在 Mac 本地完整部署 Qwen3.8-27B 并实测体验。Q4 量化仅需 15G 内存，Mac 无显卡也能跑出 22 TPS，真正开启无限 token 时代。

## 性能数据

| 指标 | 数值 |
|------|------|
| 测试设备 | 10核 MacBook Pro（无独立显卡） |
| Q4 量化推理速度 | ~22 TPS（录频有损失，实际略快） |
| MTPLX 优化版本（满血 MBP） | ~60-65 TPS |
| 长上下文平均 TPS | ~16 |
| Prefill TPS | ~157 |
| 缓存命中率 | 最高 80%+ |
| 内存占用 | ~15G + 上下文 |

## 硬件需求

- **最低**：15G 内存（无显卡 Mac 可跑）
- **推荐**：24GB 显存显卡（RTX 3090/4090）
- **最佳体验**：满血 MacBook Pro + MTPLX 优化版本

## 模型选择

推荐模型：**Youssofal/Qwen3.8-27B-MTPLX-Bare-Speed**
- 针对 Apple Silicon 性能优化
- 满血 MBP 可达 60+ TPS

其他选项：
- Unsloth GGUF 量化版本（下载量大）
- 32G 内存最佳尺寸：`UD-Q4_K-XL.gguf`

Hugging Face 衍生模型约 1500 个，开源不到一周社区已非常丰富。

## 下载方式

- Hugging Face：https://huggingface.co
- 国内镜像：https://hf-mirror.com（下载更稳定）
- 模型权重约 17G，网速快则半小时搞定

## 部署方式

两种主要方式，均有桌面端客户端：

1. **MTPLX** — 原生 Apple Silicon 优化
2. **Ollama** — 通用方案，社区更成熟

部署后默认暴露 OpenAI / Anthropic 格式端口，密钥留空，模型名称随意填，可接入任意 harness。

## 无设备也能跑：Kaggle 方案

Kaggle 每个用户每周有 **30 小时双卡 T4** 额度：

- Notebook：https://www.kaggle.com/code/siligrove/qwen3-8-27b
- Fork 后按步骤执行，无需本地设备即可跑 Qwen3.8

## 多模态能力

- AA 评分 52，与总参数 200B+ 的 DS Flash 0831 同分
- 支持图片理解、文档 OCR 解析、图片相关工具
- 一个模型搞定图文，对私有化部署非常友好

## Coding 实测

- 给 dsh（Desktop Shell）撸了一个 TUI 插件
- 因安全考虑未给全权限，单独 clone 代码改造
- 耗时约一天，基础功能正常
- 说明修 bug 和功能需求没问题，有一定开发能力可玩

## 关键亮点

1. **免费无限 token** — 本地部署零成本推理
2. **无显卡门槛大幅降低** — 10核 Mac 都能跑 22 TPS
3. **多模态同级表现** — 27B 模型打出 200B+ 级别评分
4. **生态爆发** — 开源不到一周 1500+ 衍生模型
5. **Harness 兼容** — OpenAI/Anthropic 格式，无缝接入 Claude Code、Cursor 等工具

## 意义

这是半个年来首次无显卡也能本地跑 Opus 4.6 级别模型。在聊天框都变成计价框的时代，Qwen3.8-27B 的意义不仅是性能，更是让普通人重新拥有了"无限 token"的 AI 使用权。

甚至有玩笑称 Qwen3.8-27B 惊动了 Anthropic 的 Dario，要求召开紧急安全会议。
