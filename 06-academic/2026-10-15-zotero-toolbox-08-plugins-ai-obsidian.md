# 插件、AI 与 Obsidian，打通科研知识系统｜Zotero 科研工具箱 08（终篇）（知微图苑）

- **日期**：2026-10-15
- **来源**：微信公众号「知微图苑」
- **分类**：06 AI学术应用
- **原文**：https://mp.weixin.qq.com/s/V2zOD-BmAicYHeCjKZsysw
- **相关**：[Zotero + Codex + Obsidian 文献工作流](2026-08-10-zotero-codex-obsidian-literature-workflow.md)、[Obsidian 科研工作流系列 01](2026-10-14-obsidian-research-workflow-series-01.md)

## 概述

《Zotero 科研工具箱》系列终篇（全 8 期）。前七期覆盖采集→组织→阅读→引用→同步协作主链路；本期讲主链路之外的三件事：**插件、AI 集成、与 Obsidian 双向联动**，并给出系列回顾和长期路线图。

## 关键内容

### 1. 插件安装与兼容性
- Zotero 7 换了插件架构，老插件须更新；只装标注支持 Zotero 7+ 的（主流插件已适配 Zotero 10）
- 安装：下载 `.xpi` → 工具 → 插件 → 右上齿轮「从文件安装」→ 重启
- 贵精不贵多：每多一个插件多一份升级兼容风险
- 来源只认 Zotero 官方插件目录与中文社区 zotero-chinese.com——xpi 能读到文库全部数据

### 2. 三款硬核插件
| 插件 | 作用 |
|---|---|
| **Better BibTeX** | 稳定不变的引用键（如 `acemoglu2020robots`），改标题键不变；自动导出 `.bib`，文库更新 LaTeX 项目同步。写 LaTeX 必装 |
| **Zotero Style Plugin** | 软件内搜索、预览、安装上万种 CSL 样式，换期刊格式快 |
| **Zotero PDF Translate** | 阅读器内选中即译、对照阅读、引擎可配；提醒：关键术语和论证回原文，别长期靠整段中译 |

### 3. AI 插件三种接法
- **自带模型额度型**：PapersGPT——内置聊天，也可接 GPT-5 / Claude / Gemini / DeepSeek，对单篇或整个集合提问、总结、对比、找研究缺口
- **免费开源型**：AIdea——2026 年起支持免 API Key 的 OAuth 登录，零成本入门
- **MCP 协议型**：MCP for Zotero——Claude、ChatGPT 等外部 AI 客户端通过 MCP 直接读文库，无需导出上传

**适合交给 AI**：摸清陌生文献骨架、跨多篇对比、逼问研究空白。
**别交给 AI**：直接采信它给的数字和引用（会编造文献，事实必回原文核对）；代替你形成观点（AI 结论是平均数）。当研究助理，不当第二作者。

### 4. Zotero ↔ Obsidian 双向联动（重头戏）
- 分工：**Zotero 管文献**（题录、PDF、标注、引用，权威来源）；**Obsidian 管思考**（文献笔记、主题笔记双链、写作草稿）
- 架桥插件：
  - **ZotLit**：功能最全，支持多文库，Obsidian 内一键拉取题录+全部标注并套笔记模板，已适配 Zotero 10
  - **Zotero Citations**：偏引用管理，脚注/尾注，笔记可导出 Word
  - **Obsidian Notepad for Zotero**：反向——在 Zotero 条目面板里直接编辑对应 Obsidian 笔记
- 跑通后的日常：Zotero 精读标注 → Obsidian 自动生成结构化文献笔记 → 写自己的理解并双链挂到主题 → 写论文时主题笔记长成段落，引用经 Better BibTeX 键指回 Zotero 原文

### 5. 系列闭环
01 认知、02 入门（地基）→ 03 采集 → 04 组织 → 05 阅读 → 06 引用 → 07 同步协作 → 08 进阶联动 → 论文产出

### 6. 长期路线图
- **第 1 个月**：装好 Zotero，把开题文献收齐、去重；别折腾插件
- **第 2—3 个月**：建集合/标签规则，开始精读，养成「标注 + 半句注释」习惯，跑顺同步与备份
- **动笔阶段**：Word 插件边写边引，定期用检查清单核对题录
- **有余力时**：再上 Better BibTeX、AI、Obsidian 联动——工具服务于阶段

## 本期清单
1. 插件只装支持 Zotero 7+ 的，来源认准官方目录和中文社区
2. 硬核三款：Better BibTeX、Style Plugin、PDF Translate
3. AI 三种接法；事实回原文核对，观点自己形成
4. Zotero 管文献，Obsidian 管思考，ZotLit 等插件架桥
5. 按阶段上工具，第一个月不折腾插件

## 备注
- 插件版本兼容情况（Zotero 10 适配、AIdea OAuth）为作者 2026 年描述，安装前以插件页说明为准。
