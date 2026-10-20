# 科研 Skill 怎么选：按六类任务整理 11 个 GitHub 项目，「装十个不如用熟两个」

- **日期**：2026-10-20（收藏）
- **来源**：微信公众号「ZenResearch · 禅意科研」（MedAI Toolbox 栏目，面向医学科研）
- **原标题**：科研人别再瞎装 Skill 了：GitHub 上这些科研 Skill，真正值得装的只有这一批
- **分类**：06-academic（AI 学术应用 / 科研 Skill 选型）
- **URL**：https://mp.weixin.qq.com/s/0JLeEuAzknfAe9ZjL3trMQ
- **核对日期**：作者按各仓库 README、源码和许可证于 2026-10-05 整理，**没有运行验证全部功能**；六类划分和搭配是编辑建议，不是评测排名。
- **相关笔记**：[Supervisor-Skills](2026-10-20-supervisor-skills-hkust-research-skills.md)（同日）、[8 月 Top 10 科研 Skill](2026-08-11-github-top10-research-skills.md)、[9 月科研 AI Star 榜](2026-09-07-github-research-ai-stars-sept.md)、[科研绘图 Skill Top 10](../02-tools/2026-08-10-research-figure-skills-top10.md)

## 核心观点

同类 Skill 装太多（五六套写作、三四套检索、两套全流程 Agent），不会让模型变聪明，反而带来**触发冲突、上下文冗余、调用混乱和维护负担**。先想清楚卡在哪一步再装。

## 一张表：卡在哪一步就看哪个

| 卡在哪 | 推荐 | 仓库 |
|---|---|---|
| 想把「查文献—写作—审稿—修改」串起来 | **Academic Research Skills** | Imbad0202/academic-research-skills |
| 让 Agent 自动跑机器学习实验 | **ARIS**（Auto-Research-In-Sleep） | wanshuiyin/Auto-claude-code-research-in-sleep |
| 找选题、判断创新性 | **Supervisor-Skills** | HKUSTDial/Supervisor-Skills |
| 从优秀论文的论证结构反推写法 | **PaperSpine** | WUBING2023/PaperSpine |
| 生物、化学、药物、组学数据库调用 | **Scientific Agent Skills** | K-Dense-AI/scientific-agent-skills |
| 实证研究、因果推断、经济学/社科方法 | **Auto-Empirical Research Skills** | brycewang-stanford/Auto-Empirical-Research-Skills |
| 流程图、架构图、可编辑 Draw.io 图 | **Academic Figure Generator** | dongzhuoyao/academic-figure-generator |
| Nature 风格写作、科研绘图、论文工作流 | **Nature Skills** | Yuan1z0825/nature-skills |
| ML/AI/CV/NLP 论文写作 | **AI Research Writing Skill** | jin-s13/ai-research-writing-skill |
| AI 研究工程完整技能库 | **AI Research Skills**（Orchestra） | Orchestra-Research/AI-Research-SKILLs |
| 投稿前深度检索、信源核查 | **Deep Research Skill** | ravmike/deep-research-skill |

## 六类项目要点

**1. 全流程自动科研 Agent**（功能重叠多，选一个主力就够）
- **Academic Research Skills**：research → write → review → revise → finalize 全链路；约 5 万 Star（10-05）。适合文献综述、背景梳理、框架、初稿、模拟审稿和反复修改。医学科研以「文献 + 设计 + 写作」为主时，比 ARIS 更容易进入日常工作。
- **ARIS**：面向 ML 自动实验循环，覆盖 idea discovery、检索、实验设计、GPU 实验执行、结果分析、写作、rebuttal、多轮 review。特色是 **cross-model review**：一个模型执行、另一个当 reviewer 挑问题。适合医学影像分类/分割、多模态、foundation model、消融实验；传统回顾性临床研究（Cox 回归、病例对照）没必要硬上。

**2. 选题与创新点**
- **Supervisor-Skills**：AI 副导师，把「问题定义 → 文献位置 → 现有方法不足 → 你能补什么」讲清楚。适合有数据但没形成研究问题、想法模糊、写到一半发现创新性不够、不会在 Introduction 里建立研究空白的人。
- **PaperSpine**：motivation-driven，先找论文的「脊柱」再填正文——为什么值得做、怎么建立空白、为什么用这个方法、每个实验支撑哪个 claim。PaperSpine5 已扩展到检索、全文写作、配图、引用核验、review、Word/LaTeX/PDF 交付，但首次安装要按上游说明获取完整平台套件。读顶刊论文时，与其让 AI 总结摘要，不如问它每一段完成了什么逻辑任务。

**3. 跨学科 AI for Science**
- **Scientific Agent Skills**：一百多个科学技能，覆盖生物信息、基因组、**单细胞**、药物发现、化学信息学、PK/PD、分子对接、微生物组，整合 TCGA、GEO、ChEMBL、PubChem 等数据库。价值在于把「去哪查、API 怎么调、工具怎么用」提前包装好。
- **Auto-Empirical Research Skills**：实证研究，覆盖 DID、RD、IV、SCM、DML、matching、稳健性、复现、数据清洗、表格和图、投稿流程。值得借鉴的设计：**用 router 按任务只调用当前需要的那个子技能**，不把所有 Skill 都加载进上下文。适合卫生政策、健康经济学、医保数据库、自然实验。

**4. 科研绘图**（至少三种任务：数据图如 ROC/KM/森林图；方法图如流程/网络结构/pipeline；graphical abstract 和机制图）
- **Academic Figure Generator**：Python 工具（快照里没有 SKILL.md），可从草图、文字 prompt 或 DSL 生成**可编辑的 draw.io** 方法图，再导出 PNG/SVG/PDF。适合方法流程、架构、技术路线图，**不做统计结果图**。多面板数据图用 Nature Skills 的 nature-figure 更合适。

**5. 顶刊写作与排版**
- **Nature Skills**：社区项目，**不是 Nature 官方**，不保证达到任何期刊的标准。包括 Nature 风格写作、润色、读论文、科研图、proposal、data availability、paper-to-PPT，核心是 nature-figure 和 nature-polishing。建议用在边界明确的任务上：查 Results 有没有 claim drift、统一单位和术语、查 Discussion 是否过度解释、做投稿级多面板图、全文术语一致性检查。
- **AI Research Writing Skill**：「Claim-evidence engineering, not prose generation」，每个主要 claim 要能追溯到代码、实验日志、笔记或核验过的引用。偏 ML/AI/CV/NLP。

**6. 可信科研助手**
- **AI Research Skills（Orchestra）**：AI 研究工程全阶段（survey、ideation、架构、微调、后训练、评估、RAG、多模态、可解释性、MLOps、写作）。做传统临床研究就别因为它「全」而装。
- **Deep Research Skill**：Scope → Plan → Retrieve → Triangulate → Synthesize → Critique → Refine → Package。重点是多源检索、信源可信度评估、引用追踪、交叉验证。适合放在流程最前面和最后一道审核。例如要写「目前尚无研究系统评估……」之前，先去 PubMed、Scholar/OpenAlex、arXiv、指南、原始论文查清楚。

## 它们重叠在哪

五个都涉及「论文写作」，但角色不同：

| 项目 | 更像什么 |
|---|---|
| Academic Research Skills | 全流程科研写作流水线 |
| Supervisor-Skills | AI 副导师 / 研究方法 |
| PaperSpine | 论文论证结构与证据主线 |
| Nature Skills | 写作、润色、图形与投稿级 QA |
| AI Research Writing Skill | Claim–Evidence 对齐 |

五个全装、让 Agent 自己选，未必比只留两套好。

## 医学科研搭配建议（按功能划分，未经运行评测）

- **A 普通临床科研**（回顾性队列、生存分析、临床数据库）：Supervisor-Skills + Nature Skills + Deep Research Skill，分别管研究问题与方法、写作图形与全文 QA、文献与信源核查。
- **B 医学影像 AI / 深度学习**：ARIS + AI Research Writing Skill + Nature Skills；大量用生物医学数据库时再加 Scientific Agent Skills。
- **C 只想提高写作**：Academic Research Skills + Nature Skills，先用熟一套，有明确缺口再补。

## 装之前问自己 5 个问题

1. 它解决的是不是我**现在真正存在**的问题？
2. 我是不是**已经装过同类**？别因为 Star 多 5000 就再装一套。
3. 它有没有**代码执行、脚本、外部依赖**（Python、shell、外部 API、装依赖、读本地文件）？装前至少读一遍 SKILL.md、README、安装脚本和权限边界。
4. 能不能**项目级安装**，而不是全局安装？项目需要什么就在项目里装什么。
5. 用完以后**结果能不能核查**？文献能否找到、DOI 对不对、数字能否追溯、表格是否来自真实数据、结论有没有越过证据边界。

## 许可证（原文核对）

- Academic Research Skills：**CC BY-NC 4.0**；Supervisor-Skills：**CC BY-NC-SA 4.0**（非商业、相同方式共享）。
- Scientific Agent Skills、Auto-Empirical Research Skills 部分子项许可不明或条款不同；Academic Figure Generator、Deep Research Skill 没找到明确的再分发许可。其余以仓库实际文件为准。

## 备注：对照本机

- 本机 Claude Code 已装 **academic-research-skills 插件**（ARS v3.23.0，有 v3.23.1 可更新），也就是这篇的第 1 项，相当于「配置 C」的主力已经有了。
- 和 Dejian 的单细胞、bulk RNA-seq 工作最相关的是 **Scientific Agent Skills**（单细胞、GEO、TCGA、基因组）。如果要试，按第 4 条建议，只在具体项目目录里项目级安装其中几个子技能，并且先审一遍脚本。
- 原文末尾的网盘资源包和公众号引流已略去；资源包本身也说明它不是一键安装包。
