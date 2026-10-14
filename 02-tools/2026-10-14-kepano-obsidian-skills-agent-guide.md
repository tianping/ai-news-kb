# obsidian-skills：Obsidian CEO 维护的官方 Agent 技能集，装 5 份说明书 AI 就懂 Obsidian

- **日期**：2026-10-14
- **来源**：微信公众号「盍曰AI」（Obsidian × AI 系列第 10 篇）
- **分类**：02 工具与产品（Agent Skills / Obsidian）
- **原文**：https://mp.weixin.qq.com/s/iwQ1MFcYO6OI6FawJm3y3w
- **仓库**：https://github.com/kepano/obsidian-skills （MIT）

## 概述

AI 不懂 Obsidian 的规矩：批量补内链写成 `[标题](另一篇.md)` 普通 Markdown 链接（能跳但图谱、反链面板不认），callout `> [!WARNING]` 写成普通引用，不知道 Bases 是什么。解法是 **kepano（Steph Ango，Obsidian CEO、Minimal 主题作者）**亲自维护的 **obsidian-skills**：一组遵循开放 Agent Skills 规范的 `SKILL.md` 说明书，装在 **Agent 一侧**（Claude Code、Codex CLI、OpenCode 等），Agent 干活前先读规矩再写文件，**不改动 Obsidian 本身**。

作者实际清点：目前 **5 个技能**（有草稿称"7 个 Workflow"不准；仓库仍在活跃更新，装时以实际为准）。

## 与社区插件的区别

| | obsidian-skills | 社区插件 |
|---|---|---|
| 装在哪 | Agent 侧（`.claude/skills/` 等） | Obsidian 第三方插件目录 |
| 改不改 Obsidian | 不改，纯说明书 | 注入代码、改 UI |
| 卸载 | 删文件夹 | 插件面板禁用/卸载 |
| 谁在用 | Claude Code / Codex / OpenCode | Obsidian 本体 |
| 许可 | MIT | 各家不同 |

## 5 个技能

| 技能 | 管什么 | 典型场景 |
|---|---|---|
| obsidian-markdown | Obsidian 风味 Markdown：wikilink、嵌入、callout、frontmatter 属性 | 补内链、写日报，链接格式一次到位 |
| obsidian-bases | `.base` 文件：视图、筛选、公式 | 生成"按状态分组的项目库" |
| json-canvas | `.canvas` 的 JSON Canvas 规范：节点、连线、分组 | 会议纪要画成依赖图 |
| obsidian-cli | 官方 CLI 的 115 个命令 | 让 Agent 挪文件不断链 |
| defuddle | 网页 → 干净 Markdown（去广告导航） | 剪藏网页进库，省 token |

装完变化：内链全是 `[[双链]]`，frontmatter 的 tags 写成数组（Dataview 和 Bases 都能查），callout 语法正确——"开始像 Obsidian 用户一样写文件，而不是像程序员一样"。

## 安装三条路

- **A. Claude Code 插件市场**（在聊天框里，不是终端）：
  ```
  /plugin marketplace add kepano/obsidian-skills
  /plugin install obsidian@obsidian-skills
  ```
- **B. npx 交互安装**（任何 Agent）：`npx skills add https://github.com/kepano/obsidian-skills`，空格勾选技能（作者建议全选），再选装到哪些 Agent（列表 73 个）
- **C. 手动拷文件**：Claude Code 放项目 `.claude/skills/` 或全局 `~/.claude/skills/`；Codex `~/.codex/skills/`；OpenCode `~/.opencode/skills/`

装完无需配置。**渐进披露**：技能平时不占上下文，提到相关话题（如"帮我建个 Base"）才读对应 SKILL.md——不提需求就不加载，是特性不是故障。

## 四个现场

1. **补内链**：给读书笔记的人名补双链 → 输出 `[[西奥迪尼]]`，并列出暂无目标笔记的断链
2. **Bases**：筛 tags 含 project 且 status ≠ archived、按 status 分组 → 直接吐出合法 `.base` 文件
3. **Canvas**：读 `[[Q1 规划]]` 画依赖 → 带坐标、连线、卡片嵌入的 `.canvas`
4. **defuddle 剪藏**：剥到只剩正文再转 Markdown，作者经验比直接抓 HTML 省约 70% 上下文；`.md` 结尾网址本身就是 Markdown，直接抓即可

## 自己写一个技能

一个文件夹 + 一个 `SKILL.md`，frontmatter 里 `name` 和 `description` 最关键，**description 写清触发词比写功能介绍重要**：

```markdown
---
name: daily-report
description: 生成每日工作日报。Use when the user asks for 日报/周报。
---
# 日报生成规范
- 路径固定为 报表/YYYY-MM-DD-日报.md
- frontmatter 必须含 date、tags: [日报]、mood
- 正文三段：今日完成、明日计划、阻塞项
- 用 wikilink 链接提到的项目笔记
```

存到 `.claude/skills/daily-report/SKILL.md`。schema 先定死、AI 只往格子里填，就不会格式漂移。

## 写库三条铁律

1. **先拿沙箱库练手**：复制一个空 vault 试跑，确认 Obsidian 打开不报错再上正库（`.base` 写错一个逗号视图就白屏）
2. **写操作前备份**：超过 10 个文件的写操作先整库 `cp -r`；技能只是说明书，Agent 的写权限要自己管
3. **别混淆"官方"**：它是 Obsidian CEO 个人维护的官方仓库，但不是 Obsidian 软件的一部分，也不在插件市场里

## 备注

- 系列作者的框架：Obsidian 与 AI 的三条通道——**MCP 是数据口、CLI 是遥控器、skills 是说明书**。
- 与本库 [Claudian 插件](2026-10-14-claudian-obsidian-agent-plugin.md) 互补：Claudian 把 Agent 放进 Obsidian 界面，obsidian-skills 教 Agent 写对 Obsidian 格式，两者可同时使用。另见 [claude-obsidian](2026-10-07-claude-obsidian-knowledge-graph.md)、[科研人的 Obsidian 实战手册](../06-academic/2026-10-14-obsidian-research-workflow-series-01.md)。
