# wigolo：不要 API Key 的本地 Agent 联网神器

> 来源：微信公众号「极客之家」，2026-08
> 原文：https://mp.weixin.qq.com/s/RiMdKJGEFY8AmNQvDXWtyA
> 开源：https://github.com/KnockOutEZ/wigolo（AGPL 协议，公开 beta）

## 是什么

wigolo 是给 AI Agent 提供网页能力的工具。以 MCP Server 形式接入 Claude Code、Cursor、Codex、Gemini CLI 等，也支持 REST API 和 TypeScript/Python SDK，可接 n8n 自动化。

**核心卖点**：搜索、抓取、爬取全部跑在本地，**不需要 API Key**，不按次收费，没有额度限制。查询记录不出本机，除非主动接外部模型做报告汇总。作者单人维护，7000+ 测试用例。

## 内置 10 个工具

| 工具 | 功能 |
|------|------|
| search | 并行查 18 个公开搜索引擎，结果融合排序 + 本地模型重排；每条带原文摘录、引用 ID 和打分明细 |
| fetch | 单页抓取：HTTP 优先，反爬/SPA 自动升级无头浏览器；输出干净 Markdown + 元数据；可点击/输入/滚动/截图；遇验证码明确标记 `blocked_by_challenge` |
| crawl | 多页爬取，BFS/DFS/sitemap 模式，默认遵守 robots.txt，按域名限速，过滤重复样板内容 |
| extract | 从页面抽结构化数据：表格、JSON-LD、文章、商品；支持自定义 JSON Schema |
| cache | 所有查询进本地缓存（`~/.wigolo/`），支持关键词+语义混合检索，断网也能用 |
| find_similar | 给 URL 或概念，三路合并找相似（关键词+语义+实时网页） |
| research | 拆解问题→并发搜索→生成带引用报告（需配 LLM；不配则返回原始材料由 Agent 组装） |
| agent | 自主规划步骤：搜索→抓取→提取一步步做完，可设时间限制 |
| diff | 对比页面与上次访问的差异，列出改动 |
| watch | 定时复查页面，变化时推送 webhook（盯文档更新、价格变动） |

## 安装与使用

```bash
# 基础安装（自动下载浏览器引擎和本地模型）
npx wigolo init

# 同时接入 Agent 工具
npx wigolo init --agents=claude-code,cursor

# 健康检查（哪个组件有问题、修复命令一并列出）
npx wigolo doctor

# 起本地 REST 服务（接 n8n 等自动化工具）
npx wigolo serve
```

**要求**：Node 20+，磁盘约 1.5GB，macOS/Linux/Windows 均支持。

**LLM 配置**：search/fetch/crawl/extract 无需 LLM；research/agent 需配语言模型，官方支持多家，图省事可用本地 Ollama。

## 适用场景

- Agent 查最新文档：不用凭训练数据猜，直接读官方文档，返回带原文位置可核对
- 项目更新日志监控：watch 挂上，有动静自动推 webhook
- 自动化流程接本地服务：`wigolo serve` 起 REST，n8n 直接 curl
- 省钱：Agent 一次查询可能并发十几个，按量计费云服务很贵，wigolo 零成本且有缓存

## 与同类对比

与 Firecrawl 等云端 API 工具相比：

| 维度 | wigolo | Firecrawl 等 |
|------|--------|-------------|
| 数据位置 | 本地（~/.wigolo/） | 云端 |
| API Key | 不需要 | 需要 |
| 收费 | 免费 | 免费额度有限，用完付费 |
| 开源协议 | AGPL | — |
| 复杂抓取成熟度 | beta 阶段，个别场景不如付费工具 | 更成熟 |

作者在公众号附了一组实测对比：同一 Claude 会话里，内置 WebSearch、wigolo、Tavily、Exa 四个工具跑同一问题，结论一致、头号信源一致，但 **wigolo 是唯一返回字节级定位证据的**，打分和引擎状态全部列出。爬取能力反而是它的强项。

## 几点实在话

- 磁盘 1.5GB 省不了（本地模型 + 浏览器引擎）
- 18 个引擎融合，单个挂了影响不大，公共搜索偶发问题正常
- beta 阶段，个别复杂抓取场景不如成熟付费服务
- AGPL 协议，不用担心哪天突然开始收费
