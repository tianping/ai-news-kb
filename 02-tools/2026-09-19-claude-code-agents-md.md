# Anthropic 接受 OpenAI 标准:Claude Code 开始支持 AGENTS.md,智能体共用项目指令

## 元数据
- 日期:2026-09-19
- 来源:机器之心(编辑:泽南、杨文)
- 分类:02-tools(AI 编程工具)
- URL:https://mp.weixin.qq.com/s/u3oTtbEFuTUAPWsEYZSXkw

## 概述
Claude Code 从 2.1.277 版本开始支持跨厂商项目指令标准 AGENTS.md(源自 OpenAI Codex)。这标志着主流 AI coding 工具围绕统一的 "Agent README"(README for agents) 收敛,Anthropic 与 OpenAI 在竞争激烈的 Agent 赛道走向兼容。

## 关键内容

**核心更新**
- Claude Code 成员 Thariq Shihipar 发帖:Claude Code 开始支持 AGENTS.md。
- 从 2.1.277 版本起,若项目目录没有 CLAUDE.md,Claude Code 会检查并使用 AGENTS.md;用户可通过 /config 切换行为。
- 此能力建立在 mods 机制上,Anthropic 已将 agents-md 做成内置 mod 并公开源码:
  - 源码:https://github.com/anthropics/claude-code/tree/main/mods/agents-md
- AGENTS.md 像"项目写给 AI Agent 看的说明书",官方称为「README for agents」,内容含项目结构、装依赖、跑哪些测试、代码规范等。

**加载规则**
- 默认模式:沿项目目录树查找 AGENTS.md,只要项目路径无项目级 CLAUDE.md,找到即作为项目指令加载。
- Agent 读取子目录文件时,mod 会按目录层级补充对应 AGENTS.md;同级有 CLAUDE.md 则遵循原规则。
- /config → Project instructions 三种模式:仅 CLAUDE.md / 无 CLAUDE.md 时回退 AGENTS.md / 同时加载两类。

**行业背景:Shopify 的"复杂性税"**
- Shopify CEO Tobi Lütke 曾考虑禁用 Claude Code,直到其支持读取 AGENTS.md、.agents/skills 等文件。
- 团队成员混用 Codex、Cursor、Claude Code 时,同一仓库出现不同配置入口(Codex 读 AGENTS.md,Claude Code 读 CLAUDE.md),文件差异导致不同 Agent 获得不同项目规则。
- 旧方案(软链接、在 CLAUDE.md 写 @AGENTS.md)在小项目可行,大型 monorepo 中配置文件沿目录树递归生效,维护成本高。Lütke 称之为「复杂性税」。

**Anthropic 为何此前坚持 CLAUDE.md**
- Thariq 解释:不同模型家族行为特点不同,system prompt 显著影响模型表现;Claude 模型对 skills、system prompt、CLAUDE.md 的组织方式有特定偏好,Claude Code 也会针对不同模型调整 system prompt。
- 呼应 Anthropic 关于 context engineering 的讨论:项目说明、技能文件、system prompt 共同构成 Agent 完成任务所需上下文。
- 模型专属设计优化 Claude Code 体验,但增加跨工具协作维护成本。

**兼容≠统一(局限)**
- 默认模式仅在无 CLAUDE.md 时才回退读 AGENTS.md;同时加载需在 /config 调整。
- .agents/skills 与 .claude/skills 仍是两套独立目录。
- 团队仍需决定哪些规则为跨工具共同内容、哪些只服务 Claude Code。

## 影响与备注
- 项目指令文件正从"工具偏好"变成"代码库基础设施"——跨工具共享项目上下文已成开发团队现实需求。
- Claude Code 曾是 AGENTS.md 标准最大缺席者之一,如今主流 AI coding 工具围绕「Agent README」明显收敛,不同 harness 间迁移成本将持续降低。
- OpenAI Codex 负责人 Tibo 祝贺:"这就是对的,欢迎来到光明的一边。"

## 参考链接
- https://x.com/trq212/status/2101009392611278961 (Thariq 公告帖)
- https://github.com/anthropics/claude-code/tree/main/mods/agents-md (agents-md mod 源码)