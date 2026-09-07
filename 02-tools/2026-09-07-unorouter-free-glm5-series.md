# Uno Router 免费开放 GLM-5.2 / GLM-5.3 系列模型：搜索+推理版全部 $0

- **来源**: [微信公众号文章](https://mp.weixin.qq.com/s/Bo0IhiWmgM0iJaZ5Fa9C5g)
- **日期**: 2026-09-07
- **平台**: [Uno Router](https://api.unorouter.com) — AI 模型 API 聚合网关
- **核心卖点**: 注册即调，输入/输出/缓存均 $0，1M token 超长上下文

## 免费模型清单（6 款）

| 模型 | 特点 | 输入 | 输出 | 缓存 | 上下文 |
|------|------|------|------|------|--------|
| glm-5.2-search | 搜索增强 | $0 | $0 | $0 | 1M |
| glm-5.2-think-search | 推理+搜索 | $0 | $0 | $0 | 1M |
| glm-5.2-thinking | 深度推理 | $0 | $0 | $0 | 1M |
| glm-5.3-search | 搜索增强 | $0 | $0 | $0 | 1M |
| glm-5.3-flash-search | 轻量+搜索 | $0 | $0 | $0 | 1M |
| glm-5.3-flash-think-search | 轻量+推理+搜索 | $0 | $0 | $0 | 1M |
| glm-5.3-flash-thinking | 轻量深度推理 | $0 | $0 | $0 | 1M |

覆盖两代 GLM（5.2/5.3）、搜索与推理双增强、旗舰与 Flash 轻量版都有免费档。

## 付费档参考（裸版，无 search/think）

| 模型 | 输入/1M | 输出/1M | 缓存/1M |
|------|---------|---------|---------|
| glm-5.3-flash | $0.0108 | $0.036 | $0.00216 |
| glm-5.3 | $0.05096 | $0.160162 | $0.00946 |
| 比官方直连便宜不少，免费档不够用时切付费档也划算 |

## 接入方式（OpenAI 兼容）

1. 注册 api.unorouter.com，拿 API Key
2. Base URL 指向 `https://api.unorouter.com/v1`
3. model 填免费模型名，如 `glm-5.3-flash-thinking`

示例配置：
```json
{
  "base_url": "https://api.unorouter.com/v1",
  "model": "glm-5.3-flash-thinking",
  "api_key": "你的Key"
}
```

## 几点提醒

- ⚠️ 免费状态可能调整（推广/验证用途），以平台当前页面为准
- ⚠️ 不带 search/think 的裸版按量计费，别把免费档模型名填错
- ⚠️ 响应时间数十秒到百秒级不等，时效敏感任务请预估好

## 适合谁

- **AI 学习者/研究者**：免费跑 GLM 5 系列实验，1M 上下文做长文档分析
- **开发者**：快速接一个能搜索、能推理的免费模型做原型
- **自动化工作流**：GLM 接入 Agent/定时任务，检索+推理一步到位
- **多模型对比团队**：一个 Key 横扫 GLM 系列，先免费后付费平滑升级
