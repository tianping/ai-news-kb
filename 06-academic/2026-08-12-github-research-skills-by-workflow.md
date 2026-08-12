# GitHub 科研 Skill 热榜：10 个项目重新定义 AI 做科研的方式

> 来源：[AI学术圆桌派](https://mp.weixin.qq.com/s/THBqWjBqy5SsGRGFotkbjw) · 2026-08-12
> 转载参考：[腾讯新闻](https://news.qq.com/rain/a/20260721A020GJ00)

## 核心结论

这套不是某一个单一仓库，而是几个不同作者的活儿拼起来，按论文从立项到汇报的真实流程排列。每个 Skill 附 GitHub 仓库地址，按需挑选安装。

> 与本站已有的「GitHub Star Top 10 科研学术 Skill 排行榜」（按 Star 排名）不同，本文按**科研流程环节**推荐 Skill 组合，两者互补。

---

## 一、选题

### sci-brain
- **地址**：https://github.com/QuantumBFS/sci-brain
- **功能**：帮你找研究方向，把模糊的"我对 XX 感兴趣"磨成具体能做的题目
- **用法**：指向 Zotero 库或 Google Scholar → 像导师一样跟你聊（抛方向、你挑你反驳、查文献确认是否有人做过）→ 生成带 BibTeX 的思路报告
- **最大价值**：在你兴奋时泼冷水——"这个 2019 年有人做过了"

---

## 二、文献

### nature-academic-search
- **地址**：https://github.com/Yuan1z0825/nature-skills/tree/main/skills/nature-academic-search
- **功能**：顶刊级文献检索，走 PubMed / Crossref / arXiv / Scopus
- **特色**：每条结果标注"直接支持 / 部分支持 / 仅背景"，导出 .nbib / .ris / .bib

### academic-research-skills（文献整合包）
- **地址**：https://github.com/YuanyuanMa03/academic-research-skills
- **功能**：把 nature-academic-search + cnki-search + gs-search 打包，支持知网和 Google Scholar

### literature-review（综述成文）
- **地址**：https://github.com/stephenlzc/bibtex-literature-review
- **功能**：从 Zotero 导出的 .bib 文件直接生成带交叉引用的 Word 综述
- **特色**：正文上标编号 [1][3-5]、自动编号参考文献、跳转链接，中文论文/开题报告/课程作业都能用

---

## 三、统筹

### academic-research-skills（科研总调度）
- **地址**：https://github.com/Imbad0202/academic-research-skills-codex
- **角色**：学术研究的"总调度" Skill，把整个科研流程（研究→写作→评审→投稿）整合成一个 Codex Skill
- **定位**：横向统筹，不自己出图/查 PubMed，而是帮你把整个研究流程串起来管
- **5 个核心工作流**：Socratic 对话磨清研究问题 → 列大纲 → 检查引用 → 模拟评审 → 跑完整 pipeline
- **与前述 nature-skills 的关系**：nature-skills 是垂直专精（一个 skill 干一件事），ARS 是横向统筹

---

## 四、出图

### nature-figure（最火的科研 Skill）
- **地址**：https://github.com/Yuan1z0825/nature-skills
- **功能**：生成规范的科研图片，省去数小时作图时间
- **特色**：两套色板、SVG 文字可编辑（导出后 Illustrator 里还能改字号）、13 种图表模板
- **硬规则**："同一方法在不同子图必须同色系"——人手调经常漏，AI 按规则来不会忘
- **图例类型**：机制示意 + 图像面板 + 定量结果 + 相关性组合、热图/注释矩阵/聚类块/发散色标、雷达对比图

### scientific-visual-skills（封面/示意图/机制图）
- **地址**：https://github.com/2023Anita/scientific-visual-skills
- **功能**：制作论文配的机制图、Graphical Abstract、期刊封面风主视觉
- **三类**：信息图、封面主视觉、论文插图
- **适合**：需要画"作用机制图""实验流程图"的场景

---

## 五、写作 + 润色

> 核心四个 Skill 均在 nature-skills 仓库内

### nature-writing
- 按 Nature 作者指南写英文初稿，不是翻译腔，是正经学术英文

### nature-polishing（★ 全套最值钱）
- **25 条规则**：从 5 篇 Nature 正刊（s41586 系列）一句一句扒出来 + 一门研究生科学写作课
- **12 步流程**：最关键的一步叫"**过度声明检测**"——你的数据只支持 A，你写成了"A 表明 B"，它专门挑这个
- **价值**：审稿人最烦过度声明，这一步救过不少稿

---

## 六、投稿

### nature-reviewer
- 投之前让 AI 当审稿人，预判会被挑什么，自己先改一轮

### nature-response
- 审稿意见十几条逐条写 Response Letter，既礼貌又硬气——该认错的认错，该回怼的给数据

### nature-data
- 把论文里"数据从哪来、怎么拿"那段说明写得清楚合规
- 检查草稿够不够格，把中文笔记转成英文投稿版，处理公开数据/限制访问数据/附录材料

### 缺的一块
- **选刊决策**和 **Cover Letter** 目前无专门 Skill，直接用 Codex 通用对话能力补：给摘要和方向，列 5 本候选刊对比 IF / 审稿周期 / scope

---

## 七、汇报

### nature-paper2ppt
- **地址**：https://github.com/Yuan1z0825/nature-skills/tree/main/skills/nature-paper2ppt
- **功能**：论文/预印本/PDF/图注/阅读笔记 → 中文 PowerPoint
- **适合**：组会、文献报告、分享、答辩前讲稿准备
- **聪明之处**：不按论文章节顺序做，而是先判断论文类型（发现类/方法类/资源类/综述类），再选叙事逻辑：
  - 发现类 → "问题 → 证据"
  - 方法类 → "问题 → 方案"
- **输出**：10-16 页，每页一个主图，深层内容放备注，生成资产清单方便查"这个图在论文哪里"

---

## 速查表

| 环节 | Skill | 仓库 |
|------|-------|------|
| 选题 | sci-brain | QuantumBFS/sci-brain |
| 文献检索 | nature-academic-search | Yuan1z0825/nature-skills |
| 文献检索(含知网) | academic-research-skills | YuanyuanMa03/academic-research-skills |
| 综述成文 | literature-review | stephenlzc/bibtex-literature-review |
| 科研统筹 | academic-research-skills (ARS) | Imbad0202/academic-research-skills-codex |
| 数据图 | nature-figure | Yuan1z0825/nature-skills |
| 封面/机制图 | scientific-visual-skills | 2023Anita/scientific-visual-skills |
| 写作 | nature-writing | Yuan1z0825/nature-skills |
| 润色 | nature-polishing | Yuan1z0825/nature-skills |
| 投稿预审 | nature-reviewer | Yuan1z0825/nature-skills |
| 回复审稿 | nature-response | Yuan1z0825/nature-skills |
| 数据声明 | nature-data | Yuan1z0825/nature-skills |
| 论文转PPT | nature-paper2ppt | Yuan1z0825/nature-skills |

---

## 一句话总结

这套东西基本涵盖论文全流程，能帮你省掉大量格式、排版、润色、引用的体力活。但**点子是不是新的、对照设得对不对、数据扎不扎实**，这些还是得你自己来。其中最好用的是 nature-skills，建议首先使用。
