# Jev 接入 Claude Code、Codex：让 Coding Agent 学会先判断再执行

> 来源：微信公众号（原文标题：Jev 接入 Claude Code、Codex：让 Coding Agent 学会先判断再执行）
> 链接：https://mp.weixin.qq.com/s/DhV5oENrcnvN_gui1stsmA
> 收录时间：2026-09-22

## 核心概念

Jev 是 TypeSafe AI 推出的**判断型 Agent**，与 Claude Code/Codex 等**执行型 Agent**形成互补：

- **Claude Code / Codex**：负责干活（写代码、调试、重构）
- **Jev**：负责判断（分析架构、识别可替代逻辑、做技术决策）

官方说法："Jev 不负责生成内容，而是专门帮助 Agent 做判断、分析和决策。"

## 基本信息

| 项目 | 详情 |
|------|------|
| 平台 | https://console.typesafe.ai |
| API Key | https://console.typesafe.ai/keys |
| 环境变量 | `TYPESAFE_API_KEY` |
| 新用户额度 | $5（约 1.2 亿 Token） |
| 成本 | 比其他模型低 40～400 倍，输出侧免费 |
| GitHub Skills | https://github.com/typesafe-ai/skills |

## 在 Claude Code 中使用

### 安装 Skill

```bash
claude plugin marketplace add typesafe-ai/skills
claude plugin install typesafe@typesafe-ai
```

### 使用方式

```
/typesafe:typesafe-ai Mac mini 我有必要买吗？请直接给判断
```

典型调用：
- 判断硬件购买时机
- 分析项目可替代复杂逻辑或弱代码
- 输出含 TypeSafe 栏，标注删除/保留/抽公共组件建议

### 更新 Skill

```bash
claude plugin marketplace update typesafe-ai
claude plugin update typesafe@typesafe-ai
# 重启 Claude Code 或运行 /reload-plugins
```

自动更新：`/plugin` → Marketplaces → typesafe-ai → 启用自动更新

## 在 Codex 等其他工具中使用

### 安装

```bash
npx skills add typesafe-ai/skills --skill typesafe-ai
```

选择支持的 Coding Agent（Codex、Cursor、Gemini CLI、OpenCode 等）

### 使用

```
/Typesafe <问题>
```

### 更新

```bash
npx skills update
```

## 提示词自动安装

发给任何支持 Skills 的 Coding Agent，能根据当前环境自动安装：

> 安装 TypeSafe 技能。如果你在 Claude Code 中，请运行 `claude plugin marketplace add typesafe-ai/skills`，然后运行 `claude plugin install typesafe@typesafe-ai`。如果你在其他 Coding Agent 中，请运行 `npx skills add typesafe-ai/skills --skill typesafe-ai` 并选择你的 Coding Agent。

## 使用体验

- 12 秒出判断结果（以 Mac mini 购买建议为例）
- 输出结构化，含 TypeSafe 分析栏
- 适合大量测试（新用户 $5 ≈ 1.2 亿 Token）

## 文章作者观点

Jev 最有意思的地方是不负责生成内容，而是专门做判断/分析/决策。最适合的玩法是配合 Claude Code、Codex 等 Coding Agent 一起用——执行与判断分离。官方已提供现成 Skill，无需自己封装 API。

后续计划：测试 Jev 在真实项目、代码重构、复杂技术决策中的效果。

## 补充：Codex 实测 Jev Choice 做 Agent 决策（2026-10-12 收录）

> 来源：微信公众号「沉默王二」（原文标题：给Codex装上Jev Skill，直接起飞。）
> 链接：https://mp.weixin.qq.com/s/obgWb4O6XGFhwlACrndGVQ
> 安装提示词与上文「提示词自动安装」相同；API Key 在 https://typesafe.ai/ 注册获取（作者称约 2 分钟），`export TYPESAFE_API_KEY=...` 写进 `.zshrc`/`.bashrc` 持久化。装好后 Codex 在涉及判断、分类、路由、抽取、排序、验证时会自动调用该 Skill。

### 实测案例：让 Jev 决定 Agent 的下一步

提示词要点：
- **state**：用户请求「帮我分析今天英伟达股价为什么上涨」，`knownFacts` 为空，可用工具 4 个
- **候选项**：`answer_directly` / `web_search` / `inspect_code` / `ask_user` / `human_review`
- **执行规则**：必须实际调用 Jev Choice API；`confidence ≥ 0.8` 才执行 Jev 的选择，否则转 `human_review`；输出模型、Token 用量、完整概率和最终动作；调用失败就停止报告，不许 Codex 自己补判断。阈值可按场景调到 0.9–0.95

Jev 返回（Codex 实际发起了 HTTP 请求）：

```json
{"model": "jev-1.13.0",
 "usage": {"input_tokens": 495, "output_tokens": 63},
 "choice": "web_search", "confidence": 0.99,
 "probabilities": {"human_review": 0, "ask_user": 0.01, "inspect_code": 0,
                   "web_search": 0.99, "answer_directly": 0}}
```

confidence 0.99 超过阈值，Codex 直接执行联网搜索。作者另演示了用 Codex + Jev 填写千问表单，称速度快、效果好。

### 三种决策类型

| 类型 | 输出 | 典型用途 |
|---|---|---|
| **Noul** | 是/否的概率（0–1） | 内容审核、垃圾信息检测；工具调用前的安全检查（高于阈值拦截） |
| **Choice** | 从预定义选项中选一个，附每项概率 | 意图路由、工具选择、任务分发（客服分流、编程 Agent 选工具） |
| **Score** | 0–1 打分 | RAG 召回片段相关性排序（替代大模型 rerank）；回答质量评估，低分重生成 |

可组合：Choice 选工具 → Noul 检查调用是否安全 → Score 给结果打分，低于阈值重来。

### 成本与性能（TypeSafe 官方数据，作者转述）

- 比同类 LLM 快 40–200 倍、便宜 40–400 倍
- 输入 $0.042 / 百万 Token，输出免费；延迟 70–500 ms
- 作者观点：意图识别 GPT-6 Astra 也能做，但 Token 消耗不是一个级别；Jev 能明显省 Codex 额度

### 其他信息

- 命名取自经济学家 William Stanley Jevons（边际效用理论）
- 网友整理的用例集（意图识别、内容分类、安全审核、优先级排序）：https://github.com/Hiwoniu/Jev-Case
- 开源复刻 **kev**：基于 Qwen3.5 + LoRA，Jared Palmer 用 Devin 编写，对齐 TypeSafe 官方 System One API，可配官方 Python SDK 本地运行（作者转述，未核实）
- 作者观点：Jev 是 System 1（直觉），大模型是 System 2（推理）——把 Agent 的「判断」和「执行」拆开；判断模型本身不难做，预计各家大模型会很快跟进

相关笔记：[Jev 模型发布](../01-models/2026-09-20-jev-system-one-model.md)、[Jev 作者访谈](../01-models/2026-09-25-jev-system-one-model-interview.md)

---
*本文仅为信息整理，不构成任何推荐。*