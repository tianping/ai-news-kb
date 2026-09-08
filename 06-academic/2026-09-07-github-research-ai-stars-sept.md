# GitHub 科研 AI 工具 Star 榜（2026-09-07 更新）：10 个项目把科研交给 Agent 自动跑

**来源：** 微信公众号文章，2026-09-07
**链接：** https://mp.weixin.qq.com/s/IwMbQgscvfCqHbFaW3oNgA
**相关：** [GitHub Star Top 10 科研学术 Skill 排行榜](2026-08-11-github-top10-research-skills.md)（8 月版，含 Nature Skills/Supervisor-Skills 等已淡出）
**标签：** #科研AI #Agent #GitHub #Skill #学术写作

---

## 核心洞察

科研 AI 正在从「聊天工具」往「科研工作流」走。以前是把论文丢给 AI 让它总结/翻译/润色；现在越来越多的开源项目把文献检索、研究设计、实验执行、结果分析、论文写作和投稿检查拆成可重复调用的环节。

**Skill 比普通提示词更有意思的地方：** 不需要每次都重新告诉 AI「我的研究是什么、接下来应该做什么、哪些地方不能乱编」，而是把这些要求提前写进工作流程里。

---

## 排行榜（按 2026-09-07 Star 数倒序）

| 排名 | 项目 | ⭐ Stars | 一句话定位 |
|------|------|----------|-----------|
| 10 | claude-scholar | 5,293 | 学术+软件开发的半自动研究助手，七阶段覆盖选题到发表 |
| 9 | AI-Researcher | 5,700 | 多 Agent 协同，从想法到论文成稿 |
| 8 | AgentLaboratory | 5,826 | 端到端自主科研工作流，文献+实验+报告三阶段 |
| 7 | AI-Scientist-v2 | 7,028 | 多分支并行探索+淘汰机制，渐近式 Agent 树搜索 |
| 6 | AI-Research-SKILLs | 12,400 | 98 个 Skills，23 类别，覆盖创意到论文输出 |
| 5 | AutoResearchClaw | 14,043 | "Chat an Idea. Get a Paper."，6 种人工介入模式 |
| 4 | AI-Scientist | 14,425 | Sakana AI 开源，LLM 完成全科研流程+同行评审 |
| 3 | ARIS | 14,901 | 82 个可组合 Skills + Research Wiki + DBLP 引用核验 |
| 2 | scientific-agent-skills | 42,375 | 165 个 Skills + 100+ 科学数据库，跨学科覆盖最广 |
| 1 | academic-research-skills | 46,600 | research→write→review→revise→finalize 五阶段全流程 |

---

## 各项目详解

### 10. claude-scholar（⭐5,293）
- **地址：** github.com/Galaxy-Dawn/claude-scholar
- **定位：** 面向学术研究和软件开发的半自动研究助手
- **运行环境：** Claude Code、Codex CLI、Kimi Code CLI、OpenCode
- **核心特点：** 七阶段覆盖（研究构思→文献搜索→Zotero 管理→论文精读→空白分析→结果分析→写作→引用核验→投稿检查→审稿回复）；Evidence Record / 主张强度 / Claim Promotion Gate 约束证据与结论关系
- **适合：** 已在用 Zotero、Obsidian 或命令行 Agent 的研究者

### 9. AI-Researcher（⭐5,700）
- **核心特点：** 多 Agent 协同完成文献综述→创意生成→算法设计与实现→实验验证→结果分析→论文手稿
- **适合：** AI/ML 研究团队，连接文献调研+算法实验+论文写作

### 8. AgentLaboratory（⭐5,826）
- **核心特点：** 三阶段（文献综述+实验+报告撰写）；支持 Python/LaTeX 后续修改；关键步骤研究者仍可介入
- **适合：** CS 研究者快速搭建实验草案、整理文献证据、生成报告初稿

### 7. AI-Scientist-v2（⭐7,028）
- **核心特点：** 渐进式 Agent 树搜索——不只沿一个方向走，而是多分支并行探索；根据实验结果推进有价值方向、淘汰效果不理想路线；保留实验代码/配置/结果供回溯
- **适合：** 关注自动化科学发现、ML 实验搜索、多分支研究规划的人

### 6. AI-Research-SKILLs（⭐12,400）
- **地址：** github.com/Orchestra-Research/AI-Research-SKILLs
- **核心特点：** 98 个 Skills，23 个类别；覆盖文献调研→选题→自动研究→ML 论文写作，也覆盖模型架构/微调/后训练/分布式训练/推理优化/评估/RAG/MLOps/可观测性
- **适合：** AI/ML 研究者和算法工程师，实验开发+模型训练+评估+会议论文写作

### 5. AutoResearchClaw（⭐14,043）
- **地址：** github.com/aiming-lab/AutoResearchClaw
- **Slogan：** "Chat an Idea. Get a Paper."
- **核心特点：** 6 种人工介入模式（全自动/质量门确认/检查点/逐步执行/共同研究/自定义）；研究想法讨论→实验基线设计→领域实验 Agent→论文协作写作→主张核验→成本控制；不同领域可接入相应实验执行环境
- **适合：** 想体验自主科研系统的人；快速验证研究想法

### 4. AI-Scientist（⭐14,425）
- **地址：** github.com/SakanaAI/AI-Scientist
- **核心特点：** Sakana AI 开源；LLM 完成文献检索→代码实验→结果分析→论文生成→同行评审；支持 Semantic Scholar/OpenAlex；提供实验模板/模型配置/成本估算/自动审查
- **适合：** ML 研究者搭建自动化实验基线、批量验证研究想法

### 3. ARIS（Auto-Research-In-Sleep，⭐14,901）
- **地址：** github.com/yourusername/arise（Auto-claude-code-research-in-sleep）
- **核心特点：** 82 个可组合 Markdown Skills；覆盖文献检索→创新性检索→研究筛选→GPU 实验→跨模型审查→论文写作→持续研究记忆；Research Wiki 保存论文/创意/实验/失败记录；引用借助 DBLP/Crossref 核验
- **适合：** ML/AI/CS 方向研究者，选题→实验→结果检查→LaTeX 论文写作

### 2. scientific-agent-skills（⭐42,375）
- **地址：** github.com/K-Dense-AI/scientific-agent-skills
- **核心特点：** 165 个整理好的 Skills；连接 100+ 科学数据库；把专业 Python 工具+科研数据库+各领域研究规范封装成 Agent 可自动发现调用的 Skill
- **覆盖领域：** 生物信息学、化学信息学、医学、药物发现、材料科学、地理空间分析、统计建模、科研绘图、文献综合、实验室自动化
- **适合：** 需要调用专业软件/数据库/计算流程的研究者；生物医药、化学、材料、物理、GIS、数据科学方向

### 1. academic-research-skills（⭐46,600）
- **地址：** github.com/Imbad0202/academic-research-skills
- **核心特点：** 五阶段（research→write→review→revise→finalize）；多智能体深度研究+系统综述+学术写作+引用核验+多视角论文审查+十阶段流程编排；材料交接+主张核验+人工检查节点防上下文丢失
- **适合：** 文献综述+论文写作+投稿前检查+返修+审稿回复全流程；研究生/教师/科研团队

---

## 选工具的建议

Star 数只反映关注度，不代表项目质量。选择时看三个维度：

1. **研究方向** — 跨学科数据库调用选 scientific-agent-skills；纯论文写作选 academic-research-skills；ML 实验自动选 AI-Scientist/ARIS
2. **Agent 环境** — claude-scholar 和 ARIS 明确支持 Codex/OpenClaw；多数项目优先 Claude Code
3. **维护状态** — 项目极早期，API 和命令可能快速变动

这份榜单更适合当一张**「科研 AI 地图」**：先知道有哪些方向，再挑真正适合自己的工具。
