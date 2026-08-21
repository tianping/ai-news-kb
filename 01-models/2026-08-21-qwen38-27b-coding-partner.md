# 这个本地模型，让我 token 自由了

> 来源：[这个本地模型，让我 token 自由了](https://mp.weixin.qq.com/s/oKURF1O3UObdkaxN2HoO9Q) · 2026-08-21

## 概述

阿里发布 Qwen3.8-27B，官方跑分媲美 Claude Opus 4.6，开源免费。作者实测装上 Mac，评测其多模态、编程、agent 场景表现，并推荐搭配 Claude Code 使用。

## Qwen3.8-27B 核心规格

- **参数**：270 亿，稠密模型
- **协议**：Apache 2.0，开源免费，商用无限制
- **上下文**：原生 262K，YaRN 可扩展到 1M（理论值，实测约 262K）
- **多模态**：原生支持图片理解、视频理解（小时级长视频）
- **KV Cache**：仅 16 层用全注意力，48 层用 Gated DeltaNet 线性注意力，KV cache 约同尺寸传统模型 1/4
- **MTP**：内置多 token 预测，速度提升接近一倍

## 性能表现

### 官方跑分（对折听）
- SWE-bench Pro：61.7（Claude Opus 4.6 Max 为 53.4）
- Terminal Bench：73.0
- LiveCodeBench：90.3
- OSWorld-Verified：84.3
- OmniDocBench：91.1

### 第三方榜单
- Artificial Analysis Agentic Index：51 分，排第 7（比前代 3.6-27B 的 28 分大幅提升）
- Agents' Last Exam：42.9%，压过 GLM-5.2 和 Seed 2.1 Pro

### 社区共识
- 日常编程和 agent 任务与云端旗舰差距极小
- 复杂长推理任务云端仍更稳

## 硬件要求

| 设备 | 体验 |
|------|------|
| 24GB 显存或 Mac 24GB 内存 | 底线入场，4bit 约 17GB |
| 32GB Mac | 舒服档，上下文可开长 |
| 48GB 以上 | 可上 8bit，接近无损 |
| 16GB 及以下 | 装不下，别折腾 |

### 速度实测
- M4 Max + MLX 4bit：~23 tok/s
- M5 Max + 优化 4bit + MTP：50-60 tok/s
- RTX 5090 + MTP：90-120 tok/s
- 作者 M5 Max 128G Ollama 4bit：~25 tok/s

## 搭配 Claude Code

Ollama 原生支持 Claude Code 接入：
```bash
ollama pull qwen3.8:27b-mlx
ollama launch claude --model qwen3.8:27b-mlx
```

- 无需 API 费用，无封号风险，代码不出家门
- 决定流畅度的是「读档」速度（重读上下文），不是生成速度
- 内存带宽比 tok/s 数字更值钱

## 踩坑提醒

1. **过度思考**：默认 xhigh 推理档位疯狂过度思考，建议调到 medium 或 low
   - Simon Willison 实测：画 SVG 圆圈，xhigh 想了 21 分钟烧两万 token；关掉推理 2 分钟搞定
2. **Token 消耗**：比 Qwen3.6 平均多约 3 倍，本地跑无所谓，调 API 计费需算清账
3. **知识截止**：自称 2026 年，追问 2024 年后具体事露馅
4. **1M 上下文**：目前仍是 PPT 数字，实测约 262K
5. **输出长度配额**：agent 跑长任务记得开大

## 适用人群

- ✅ 有 24GB+ 显卡或 32GB+ Mac，想不花 API 钱跑 Opus 级 agent coding
- ✅ 公司代码不能出门，值得认真评估
- ❌ 16GB 内存，看看热闹就好
- ❌ 日常问答写作，现有模型够用，不用换

## 结论

在 token 越来越不够用的时代，本地模型第一次摸到了「能用」的门槛。按这个速度走下去，完全够格的生产级本地模型早晚会出现——智能不用按月订阅，就住在你自己的电脑里。
