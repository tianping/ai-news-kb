# 2026 年最佳免费 LLM API 盘点

> 来源：[The best free LLM APIs in 2026: a live-probed list](https://wotai.co/blog/best-free-llm-apis) · 2026-08-10 · WotAI

## 概要

跨 16 个提供商约 110 个免费模型，但真正可用（条款允许个人使用）的约 57 个。文章按实际可用性分四个层级，基于实时探测的目录维护，死掉的免费层会被自动剔除。

## 四个层级

| 层级 | 模型数 | 含义 |
|------|--------|------|
| Tier 1: 真正免费 | ~57 | 真正的免费层，条款允许个人使用 |
| Tier 2: 免费但有限制 | ~34 | 可调用，但限速严格或仅限"评估" |
| Tier 3: 免注册 | ~12 | 匿名访问，无需注册 |
| Tier 4: 跳过 | ~4 | 免费，但条款禁止你要做的事 |

## 提供商总览

| 提供商 | 免费模型数 | 示例模型 | 限制 | 需账号 |
|--------|-----------|----------|------|--------|
| **OpenRouter** | ~23 | 多个 :free 路由 | 可用性轮换 | 是 |
| **NVIDIA NIM** | ~13 | NIM 目录 | 仅评估，40 req/min | 是 |
| **Ollama Cloud** | ~11 | GLM-4.7, Kimi K2, gpt-oss, Qwen3 | 1 并发，5 小时会话限制 | 是 |
| **Cloudflare Workers AI** | ~10 | Kimi K2, GLM-4.7, GPT-OSS, Granite 4 | 每日配额制 | 是 |
| **Google Gemini** | ~9 | Gemini 2.5 Flash, 3.x previews | 2026 条款收窄为商业用途 | 是 |
| **Groq** | ~9 | Llama 3.3, Llama 4, GPT-OSS, Qwen3 | 极快，限额宽松 | 是 |
| **Mistral** | ~7 | Large 3, Codestral, Devstral | 免费实验层 | 是 |
| **OpenCode Zen** | ~6 | DeepSeek V4 Flash, Nemotron | 促销性质，可变动 | 否 |
| **Cerebras** | ~4 | Qwen3 235B | 快速，禁止转售 key | 是 |
| **Cohere** | ~4 | Command R+, Command-A | ⚠️ 条款禁止个人使用 | 是 |
| **Kilo** | ~4 | :free 路由 | 匿名路由 | 否 |
| **HuggingFace** | ~3 | DeepSeek V4, Kimi K2.6, Qwen3 | tool-call 格式偶有问题 | 是 |
| **Z.ai / Zhipu** | ~3 | GLM-4.5, GLM-4.7 Flash | 非商业研究用途 | 是 |
| **GitHub Models** | ~2 | GPT-4.1, GPT-4o | 仅限原型开发 | 是 |
| **LLM7** | ~1 | GPT-OSS, Llama 3.1, GLM | 匿名 | 否 |
| **Pollinations** | ~1 | GPT-OSS 20B | 匿名 | 否 |

## Tier 1: 真正免费，从这里开始

- **Groq** — 速度之王，免费层跑 Llama 3.3/4、GPT-OSS、Qwen3，条款允许个人应用
- **Cerebras** — Qwen3 235B 跑得飞快，唯一规则：别转售 key
- **Mistral** — Large 3/Codestral/Devstral 免费实验层，允许个人和内部商业使用
- **OpenRouter** — 一个地方拿到最多免费路由（约 24 个 :free 标签），可用性会轮换
- **Ollama Cloud** — GLM-4.7/Kimi K2/gpt-oss/Qwen3 免费云端，1 并发 + 5 小时会话限制
- **Z.ai / Zhipu** — GLM-4.5 和 GLM-4.7 Flash 非商业研究豁免

## Tier 2: 免费但有限制

- **NVIDIA NIM** — 大目录但仅评估用途，40 req/min，适合试水不适合做后端
- **GitHub Models** — GPT-4.1/GPT-4o 免费，但条款限定为实验和原型
- **Google Gemini** — 强免费层（2.5 Flash/3.x 预览），2026 条款倾向商业而非消费端
- **Cloudflare Workers AI** — 每日配额，无反代条款但仅通用订阅条款

## Tier 3: 免注册

- **Kilo** — :free 路由匿名可用
- **Pollinations** — GPT-OSS 20B，无需账号
- **LLM7** — GPT-OSS/Llama 3.1/GLM 匿名
- **OpenCode Zen** — DeepSeek V4 Flash/Nemotron 促销匿名访问
- **OVH AI Endpoints** — 约 2 req/min/模型，无需注册

## Tier 4: 跳过（个人用途）

- **Cohere** — Command R+/Command-A 免费试用，但条款禁止"个人、家庭或日常用途"

## 实用建议

### 评估免费层的三个维度
1. **限速和 token 预算** — 40/min 评估层和宽松实验层完全不同
2. **上下文窗口** — 有的只给几千 token，有的给六位数
3. **Tool calling 和 Vision** — 约 79 个免费模型支持 tool calling，15 个支持 vision

### 多提供商聚合
- **[FreeLLMAPI](https://github.com/tashfeenahmed/freellmapi)**（开源，MIT）— 把多个免费层统一到一个 OpenAI 兼容端点后面，自动故障转移
- **让贵模型编排，便宜模型干活** — [Claude Code 编排免费 LLM 池](https://wotai.co/blog/free-llm-api-claude-orchestrator) 的思路：Claude 做决策，免费模型做苦力

## 关键提醒
- 免费层每周都在变动，具体限额以提供商文档为准
- 免费层用于学习和原型，不是稳定后端
- 上线前换成付费 API
- 79 个免费模型支持 tool calling，15 个支持 vision
