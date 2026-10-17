# Obsidian 10 个插件：打造你和 AI 共用的知识库系统（Josh乔希说）

- **日期**：2026-10-17
- **来源**：微信公众号（Josh乔希说）
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/Dj4nM67M77G-INA7nSgKAw
- **相关**：本库 [kepano/obsidian-skills Agent 指南](2026-10-14-kepano-obsidian-skills-agent-guide.md)、[Claudian：Obsidian 内的 Agent 插件](2026-10-14-claudian-obsidian-agent-plugin.md)

## 概述

把 Obsidian 当作人和 AI Agent（Codex、WorkBuddy 等）共用的"外挂记忆"，流程分三步：

1. **输入与收集**：网页、文件、和 Agent 聊出来的有用内容都进 Obsidian；
2. **分类与整理**：给笔记补上来源、主题、状态，便于查找汇总；
3. **使用**：Agent 读取整理好的笔记，辅助检索、总结、写作和安排下一步。

分工：**人负责判断和确认，插件负责输入与整理，Agent 负责把资料重组成下一次工作需要的结果。**

社区插件安装：设置 →「第三方插件」→ 关闭安全模式 →「浏览」搜索 → 安装并启用。

## 一、输入与收集

| 插件/方法 | 作用 | 要点 |
|---|---|---|
| **Obsidian Markdown Skill**（Agent） | 把有价值的 Agent 对话整理成结构化笔记存入指定文件夹 | 把 Vault 路径写进 `AGENT.md`，保存时不会找不到路径；只存会议纪要、项目决策、资料摘要、反复确认过的方法，闲聊不存 |
| **Web Clipper** | 保存网页标题、正文、链接、高亮，按模板存为 Markdown | 浏览器扩展（不是社区插件），Chrome 商店认准 obsidian.md 发布者；先建模板（标题/来源/日期/分类），先用一篇文章测试 |
| **Docxer** | 预览 .docx 并一键 Convert 成 Markdown | 转换后检查表格错位、图片丢失、标题层级与列表 |
| **OCR Extractor** | 从图片、PDF 提取文字，变成可搜索内容 | 数字、表格、专有名词易出错，重要资料要对照原图抽查；敏感资料先确认插件处理方式 |
| **Xiaohongshu Importer** | 从小红书分享链接导入标题、正文、图片、视频、标签 | 保留原文链接 + 自己的判断；平台结构变化可能导致导入失败 |

## 二、分类与整理

| 插件 | 作用 | 要点 |
|---|---|---|
| **Notebook Navigator** | 按文件夹/标签/属性浏览，卡片式预览笔记内容 | 搜索可组合，如 `.status=待处理 #工作`；把"Create new note"绑定到 Cmd/Ctrl+N，装了 Templater 可绑定"Create new note from template" |
| **Editing Toolbar** | 类 Word 格式工具栏（加粗/标题/列表/引用/链接） | 适合不熟 Markdown 的人，文件仍是 Markdown |
| **Templater** | 周报、会议纪要等固定结构做成模板 | 指定模板文件夹；Frontmatter 和变量有门槛，可让 Agent 根据已有笔记生成模板，自己测试 |
| **Dataview** | 把分散笔记汇总成表格/列表/任务看板 | 不必学查询语法，让 Agent 先检查现有字段再生成看板代码；结果不对就截图发回让它调 |

Agent 提示词示例（生成看板）：

```text
请根据"工作资料"文件夹里的笔记，生成一份 Dataview 资料看板。
显示文件名、来源、状态和最近修改时间，优先列出待处理内容。
请先检查现有字段，再输出完整代码，并告诉我放在哪篇笔记里。
```

## 三、使用：Agent 调用知识库的三个场景

1. **开会前找回项目记忆**：只让 Agent 读某个项目文件夹，输出历史决定、未解决问题、下次要确认的事项。
2. **会后把记录变成行动**：整理会议记录，区分已完成/待确认/待执行，写清负责人和截止时间，"不要补充原文没有的信息"。
3. **用旧资料写新东西**：周报、汇报、邮件先列事实再出初稿，定稿存回 Vault。

**原则**：笔记至少保留来源、主题/标签、状态、个人判断和清楚的命名；资料里没有答案就让 Agent 标出来，不让它自行补全；涉及隐私/客户/公司信息时，先只开放一个项目文件夹，写回文件保留人工确认。

## 四、辅助表达

- **Excalidraw**：在 Vault 内画流程图、草图、关系图，可把笔记拖进画布，嵌入笔记后同步更新。
- **Mind Map**：读取当前笔记标题层级，用 Markmap 生成可缩放思维导图；结构不对就回去改标题层级；可固定在侧边栏或导出 SVG。

## 新手安装顺序（不要一次装齐）

1. Web Clipper + 能访问 Vault 的 Agent 和 Obsidian Skills（建立入口）；
2. 笔记变多后装 Notebook Navigator；不熟 Markdown 再加 Editing Toolbar；
3. 按资料类型选 Docxer / OCR Extractor / Xiaohongshu Importer；
4. 固定格式笔记重复出现时，让 Agent 写模板 + 装 Templater；
5. 字段和分类稳定后，让 Agent 生成看板 + 装 Dataview；
6. 最后按需装 Excalidraw 或 Mind Map。

"插件装得越多，维护成本越高"——先解决当前问题，再决定加下一个。

## 备注

- 与本仓库的做法相通：本知识库体系也是"Agent 抓取/整理 → Markdown + 索引 → 以后检索复用"，文中"保留来源 + 状态 + 自己判断""不让 Agent 补全原文没有的内容"两条可直接沿用。
