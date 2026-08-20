# DeepSeek 涨价后，还有哪些免费模型 API 值得用？

> 来源：[DeepSeek 涨价后，还有哪些免费模型 API 值得用？](https://mp.weixin.qq.com/s/Ndq6YuOiFC1RRGbaT9vKDA) · 2026-08-20 · AI李子

## 核心结论

DeepSeek 涨价后，两个近期稳定的免费渠道：**商汤日日新 Token Plan** 和 **AMD × Cherry Studio GPU Cloud**，适合轻量任务、开发测试和备用线路。

## 渠道一：商汤日日新 Token Plan

| 模型 | 限额（5 小时窗口） |
|------|-------------------|
| SenseNova 6.8 Flash Lite | 1500 次 |
| SenseNova U1 Fast | 1500 次（生图模型，独立接口） |
| DeepSeek V4 Flash | 500 次 |
| GLM-5.2 | 500 次 |

**接入**
- 入口：https://platform.sensenova.cn/login
- 支持 OpenAI 兼容 + Anthropic Messages 兼容双协议
- Base URL：`https://token.sensenova.cn/v1`（OpenAI）/ `https://token.sensenova.cn`（Anthropic）
- ⚠️ 不支持 OpenAI Responses API；U1 Fast 生图需独立接口配置

## 渠道二：AMD × Cherry Studio GPU Cloud

- 额度：每天 $10
- 入口：https://developer.amd.com.cn/radeon/tokenfactory?source=cherry-studio
- 模型：deepseek-v4-flash-0731（主推），另有部分端侧模型
- 仅支持 OpenAI Chat Completions，不支持 Responses API
- Base URL：`https://developer.amd.com.cn/radeon/v1`
- ⚠️ 带 "Fireworks Credits" 标志的模型需额外申请，审核较慢

## 免费 API 使用建议

1. **不当生产服务** — 适合测试、低频调用、备用线路，不建议高并发生产环境
2. **控制上下文长度** — 过多历史对话/MCP/插件会增加请求体积，更快触发限速
3. **任务分配模型** — 轻量任务用廉价/免费模型，复杂推理/长文档/多步 Agent 交给主力模型
4. **降低试错成本** — 先用免费 API 测模型能力、框架兼容性、工作流可行性，再决定是否迁移到付费服务

> "免费 API 最有价值的地方，在于降低试错成本。"

## 关联笔记

- [商汤日日新 Token Plan：GLM-5.2 免费不限 Token](../02-tools/2026-08-20-sensenova-token-plan-glm52-free.md)
- [免费 API！DeepSeek V4 Flash 还能白嫖，AMD 日送 $10](../01-models/2026-08-17-deepseek-v4-flash-amd-free-api.md)
- [2026年最佳免费 LLM API 盘点](../02-tools/2026-08-10-best-free-llm-apis.md)
