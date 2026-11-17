# WorkBuddy + Zotero + Obsidian：自动下载、批量精读论文，搭个人科研知识库（AI前沿速递）

- **日期**：2026-11-11（收录 2026-11-17）
- **来源**：微信公众号「AI前沿速递」
- **原标题**：WorkBuddy + Zotero + Obsidian：自动下载、批量精读论文，搭建个人科研知识库
- **分类**：06-academic（文献管理 / 科研知识库）
- **URL**：https://mp.weixin.qq.com/s/lBOniAD9V-QCLs6HYr6eiQ
- **相关笔记**：`06-academic/2026-08-10-zotero-codex-obsidian-literature-workflow.md`（同思路的 Codex 版）、`06-academic/2026-10-15-zotero-toolbox-08-plugins-ai-obsidian.md`、`06-academic/2026-11-15-ai-research-skills-eight-stage-workflow.md`

## 一句话

三件套分工：**Zotero 存原始文献，Obsidian 存结构化笔记，WorkBuddy 负责执行任务、把两边连起来**。流水线是：检索 → 下载全文 → Zotero 分类 → AI 批量精读 → 写入 Obsidian → 跨论文检索分析。读过的论文不再是一次性报告，而是能一直攒、以后还能调用的知识库。

## 流程

### 01 连接 Zotero，准备知识库
- Zotero 装 **Zotero MCP 插件**，打开需要的权限，再把 MCP 服务地址填到 WorkBuddy 里。WorkBuddy 就能读文献、管分类、把文献入库。
- 在 Obsidian 新建本地仓库，建好论文笔记文件夹和 Markdown 模板 `templates/paper-template.md`。
- 模板字段：**研究问题、研究方法、数据来源、核心发现、研究局限**等。用统一模板批量生成，不用每篇重新交代要求。

### 02 自动下载论文，导入 Zotero
- 文献收集沿用作者之前测过的 **Zotero Literature Ingest Skill**：检索 → 筛选查重 → 获取全文 → 入库核验，一套跑完。
- 测试方向是「医学疾病预测」，重点看肿瘤预测，检索 2022 年至今的 5 篇论文，5 篇都拿到了开放 PDF 并存进 Zotero。
- ⚠️ 判断标准：**真正存下了能读的 PDF 才算全文获取成功**。下载不了的单独标出来，不能把存了个链接当成下载成功。

### 03 批量精读 → Obsidian 笔记
WorkBuddy 打开 Obsidian 仓库所在文件夹，提示词：

> 请处理 Zotero 中「医学疾病预测」分类下的所有论文，按照 templates/paper-template.md 的结构生成精读笔记，并保存到 notes 文件夹。无法确认的信息请明确标注。

- 每篇论文生成一个独立的 Markdown 文件，打开 Obsidian 就能看到。
- 进阶：装 **Dataview** 插件，按年份、研究方法、关键词等字段筛选论文。

### 04 基于已有文献回答科研问题
把 Obsidian 工作文件夹设为 WorkBuddy 工作空间，然后提问：

> 不同方法分别解决了癌症诊断、发病风险、复发风险或生存预测中的什么问题？有哪些实验支持，目前还存在哪些不足？请基于已有论文，按方法类别整理，每个结论标注来源，证据不足时明确说明。

- 输出按方法和研究目标归类（早期诊断 / 复发风险 / 生存预测），不是把每篇摘要再复述一遍。

## 要点 / 我的看法

- 提示词的核心约束值得照抄：**无法确认就标注、每个结论注明来源、证据不足明说**。跟 ai-research-skills 的「未报告」原则是一回事。
- 作者自己也提醒：AI 生成的笔记、结论和引用**仍需核对原文**。
- 只测了 5 篇开放获取论文，规模很小；付费墙文献、几百篇的库效果如何，文中没说。
- 工具本身可以换：WorkBuddy 换成 Claude Code / Codex 也能跑同一套，关键在 Zotero MCP + 统一模板 + 带来源约束的提问。
- 配套资料（MCP 插件、Skill、模板、提示词）要私信关键词 B956 领取，文中没有公开链接；全文带引流性质。
