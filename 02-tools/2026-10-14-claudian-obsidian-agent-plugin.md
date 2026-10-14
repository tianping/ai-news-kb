# Claudian：把 Obsidian 笔记库变成 AI Agent 的工作目录（15.5k Star）

- **日期**：2026-10-14
- **来源**：微信公众号「Nice哥聊AI」（Obsidian 实战篇第 7 篇 / 总第 46 篇）
- **分类**：02 工具与产品（Obsidian 插件 / Agent）
- **原文**：https://mp.weixin.qq.com/s/ECAAgrV4TFB6w0mM6kAVUA
- **仓库**：https://github.com/YishenTu/claudian （MIT；Obsidian 社区插件目录搜 Claudian 可装）

## 概述

Claudian 是一个 Obsidian 插件，一句话："**你的库就是 Agent 的工作目录**"。它不是聊天侧栏，而是让 Agent 直接在库里读写文件、搜索、跑命令、执行多步工作流，省掉"把 AI 输出复制粘贴回笔记"的最后一公里。它本身不是 AI，是个壳，调用本机已装好的 CLI 引擎。

## 安装前提

1. **仅桌面端**（macOS / Linux / Windows），手机平板不行
2. **Obsidian v1.13.0+**
3. **本机先装一个 CLI 引擎**（五选一）：Claude Code、Codex、Grok Build、OpenCode、Pi；配好订阅或 API（OpenRouter、Kimi、GLM、DeepSeek 等，需先在 CLI 里配置）

安装：设置 → 第三方插件 → 浏览 → 搜 Claudian → 安装启用。开发者可克隆到 `.obsidian/plugins` 后 `npm install && npm run build`。

## 九成人会踩的坑：CLI 路径

- 官方建议 **CLI 路径先留空**，让插件自动探测。
- GUI 应用（Obsidian）常拿不到终端里配好的 `PATH`，用 nvm / fnm / volta 的最易中招；症状：`spawn claude ENOENT` 或 `Claude CLI not found`。
- 其他情况：npm 装的 CLI 与 Node 不在同一目录（找得到 CLI 找不到 Node）；Windows 上用了 `.cmd` / `.ps1` 包装脚本，需换成 `claude.exe` 或包管理器的 `cli-wrapper.cjs`。
- 解法：查真实路径（macOS/Linux `which claude`；Windows 原生 `where.exe claude`；Windows npm 装 `npm root -g`），填入「设置 → 提供商 → Claude CLI 路径」。其他四家引擎机制相同，在各自标签页填。

## 主要设置

- **可用模型列表** vs **API 模型预览**（前者是你设定的模型，后者是供应商 API 实际可用的）
- **回复风格**：默认 / 简洁（作者推荐简洁）
- **安全模式**：acceptEdits / auto / default（不确定选 default）
- **加载用户 Claude 设置**：与 Claude Code 自身设置保持一致
- **命令与技能**：自定义斜杠命令和 Skill 触发词（对应 Claude Code 的 commands / skills）。高级项：
  - *Argument hint*：输入框占位提示，如 `[file] [focus]`，不影响 AI 逻辑
  - *Model override*：为单条命令强制指定模型（重推理用 Opus，简单摘要用 Haiku）
  - *Allowed tools*：逗号分隔限制可用工具，留空 = 全部；只读命令填 `read` 防误改
  - *Disable model invocation*：只能手动触发，防 AI 自动反复调用
  - *Run in a subagent (fork)*：独立子代理上下文，失败不污染主会话
- 建议：普通生成全默认；**批量修改 Vault 的高危 skill 开 fork + 限制 Allowed tools**；固定模型用 Model override；只想手动触发就开 Disable model invocation。

## 能干什么

- **内联编辑**（作者最看重）：选中文字或停在光标处按快捷键，直接在笔记里改，给出**逐词 diff 预览**，满意保留、不满意退回
- `@` 引用库内文件和文件夹
- `/` 调出可复用提示词模板和 Skills（用户级 / 库级）
- `/side` 或 `/btw` 开临时小对话，不影响主对话
- MCP：沿用各 CLI 自己的 MCP 配置，CLI 里配好插件即可用
- 多标签会话；双栏可常驻会话管理器

## 不绑定一家模型

支持 Claude Code、Codex、Grok、OpenCode、Pi（"更多在路上"），模型走 CLI 的配置。模型换代比插件快——换的是引擎，不动的是工作习惯；国内模型也能接。

## 三句丑话

1. **数据会发出去**：内容、附件、图片和工具输出按所选模型服务商发送（Anthropic、OpenAI、xAI 或 OpenCode/Pi 里配的那家）。但插件**不做遥测**、无后台活动，网络请求只发生在明确的模型调用、MCP 端点和 CLI 调用上。
2. **边界在"动手"**：省掉搬运和编辑，改什么、留什么、删什么仍是你的判断。
3. **门槛**：仅桌面端，需先备好 CLI 引擎。

## 备注

- 与本库 [claude-obsidian：让 Claude Code 维护 Obsidian 知识图谱](2026-10-07-claude-obsidian-knowledge-graph.md) 思路相近但定位不同：claude-obsidian 是在终端里让 Claude Code 维护库；Claudian 把 Agent 嵌进 Obsidian 界面，强调内联编辑与 diff 预览。
- 科研场景搭配见本库 [科研人的 Obsidian 实战手册（第 1 期）](../06-academic/2026-10-14-obsidian-research-workflow-series-01.md)。
- Star 数为文章发布时数据，未独立核实。
