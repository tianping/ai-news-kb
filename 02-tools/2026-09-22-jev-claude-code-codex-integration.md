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

---
*本文仅为信息整理，不构成任何推荐。*