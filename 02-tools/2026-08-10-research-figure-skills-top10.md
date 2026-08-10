# 科研绘图 Skill Top 10 榜单

> 来源：[科研绘图 Skill Top 10](https://mp.weixin.qq.com/s/hnqPsChIj2mmSCwpdp3m2w) · 2026-08-10 · 数据截至 2026-08-05，按 GitHub 仓库总星标数降序

## 一句话简介

盘点 GitHub 上最热门的 10 个科研绘图 Agent Skill 项目，覆盖论文图件生成、多面板排版、期刊投稿格式、示意图/概念图等场景。

## Top 10 榜单

### 🥇 1. Academic Research Skills
- **Star**：40,831 ⭐
- **地址**：https://github.com/Imbad0202/academic-research-skills
- **简介**：覆盖研究、写作、评审和修订的完整学术工作流，visualization_agent 专门负责论文图件
- **特点**：生成 Python Matplotlib/Seaborn 或 R ggplot2 代码，处理 APA 7.0 图注、LaTeX 插图、色盲友好配色、图件质量检查
- **适用**：已在使用完整论文工作流，希望图件与正文一致的研究者

### 🥈 2. Nature-skills
- **Star**：33,216 ⭐
- **地址**：https://github.com/Yuan1z0825/nature-skills
- **简介**：面向 Nature 系列及高影响力期刊的科研技能库，内置 nature-figure Skill
- **特点**：支持 Matplotlib/Seaborn、ggplot2/patchwork/ComplexHeatmap，绘图前写清核心结论/证据链/投稿规格，完成后检查面板编号/字体/统计标注/输出格式（SVG/PDF/TIFF）。提供 chart atlas 和真实图件资产
- **适用**：需要多面板主图、热图、森林图或严格投稿格式的生命科学与综合学科作者

### 🥉 3. Scientific Agent Skills
- **Star**：32,591 ⭐
- **地址**：https://github.com/K-Dense-AI/scientific-agent-skills
- **简介**：强调数据真实性、无障碍设计和可复现导出的通用科研可视化 Skill
- **特点**：覆盖 Matplotlib/Seaborn/Plotly，处理多面板布局、不确定性、缺失数据、颜色冗余编码与灰度可读性。检查 DPI/字体/压缩/PDF·SVG 属性/图像元数据
- **适用**：投稿级数据图、交互式探索图、多图排版

### 4. Auto-claude-code-research-in-sleep
- **地址**：https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep
- **简介**：自动化机器学习研究工作流中，把数据图和结构图拆成两项专门 Skill
- **特点**：paper-figure 从实验结果生成折线/柱状/散点/热图/箱线图/多面板图，输出 PDF/PNG；figure-spec 把结构化 JSON 转成可编辑 SVG，用于架构图/工作流/pipeline
- **适用**：ML 实验较多，需从结果批量生成图表 + 方法框架图的团队

### 5. AutoResearchClaw
- **地址**：https://github.com/aiming-lab/AutoResearchClaw
- **简介**：从研究想法推进到论文的自主研究系统，内置投稿级 scientific-visualization Skill
- **特点**：绘图规范包括期刊尺寸、矢量格式、色盲安全、灰度可辨、多面板布局、统计标注。图件与自主研究流程共享上下文
- **适用**：希望实验、写作和图件在同一自动研究系统中推进的用户

### 6. GPT-Image2-Skill
- **地址**：https://github.com/wuyoscar/GPT-Image2-Skill
- **简介**：面向 GPT Image 2 的图像生成与编辑 Skill，整理了 research-paper-figures 提示词图库
- **特点**：覆盖临床队列流程、单细胞图谱、医学AI方法图、森林图、Transformer架构、RAG pipeline 等场景。注意：涉及真实坐标/样本量/置信区间时必须由代码绘图接管
- **适用**：需要论文示意图、图形摘要、方法概念图或视觉草案的作者

### 7. MathModelAgent
- **地址**：https://github.com/jihe520/MathModelAgent
- **简介**：面向数学建模的 Agent 与 Skill 组合，兼顾数据分析图、科研模板和 Draw.io 图件
- **特点**：内置配对云雨图、交叉验证ROC、泰勒图、相关矩阵、TPE三维曲面、环形热图、Nature风格和弦图等模板，输出 PNG/PDF/SVG + 脚本
- **适用**：数学建模竞赛、课程论文、需快速复用复杂统计图模板

### 8. Figures4papers
- **地址**：https://github.com/ChenLiu-1996/figures4papers
- **简介**：以真实论文绘图脚本为主体的案例仓库，整理成可调用的科研制图 Skill
- **特点**：包含实际发表过的 Matplotlib 项目，覆盖柱状/趋势/散点/热图/复合多面板图。可直接查看数据处理、绘图函数、配色和导出代码
- **适用**：AI、计算生物学等方向，希望从真实发表图件反推代码结构的作者

### 9. codex-claude-academic-skills
- **地址**：https://github.com/zLanqing/codex-claude-academic-skills
- **简介**：面向中文科研用户的学术 Skill，scientific-toolkit-skill 负责科学计算/分析/论文配图
- **特点**：覆盖 MATLAB/Octave 与 Python 科学计算，同时导出高分辨率 PNG 和矢量 SVG，要求记录变量/单位/随机种子/命令/输出路径
- **适用**：以 MATLAB 或 Python 为主、希望用中文指令完成仿真和期刊出图的理工科研究者

### 10. AcademicForge
- **地址**：https://github.com/HughYau/AcademicForge
- **简介**：把单图规范和多面板组合拆成两层管理的学术 Skill 平台
- **特点**：figure-style 管理数据忠实性/标签密度/颜色/字体/渲染检查；figure-composer 从一句核心结论生成面板大纲，逐个渲染再拼接标注，支持逐面板修改
- **适用**：需把多个独立数据图组织成一张论文主图，并保留逐面板修改能力的研究团队

## 趋势观察

- **代码绘图为主**：Top 3 都以 Python/R 代码生成为核心，强调可复现性
- **期刊合规内建**：多月刊投稿格式检查（尺寸、DPI、字体、统计标注）已成标配
- **分流趋势**：数据图（统计可视化）与结构图（架构/流程示意图）开始走不同 Skill 线路
- **中文科研用户**：已有专门面向中文用户的 Skill（codex-claude-academic-skills）
- **AI 生成 vs 代码绘图边界**：GPT Image 2 类工具适合示意图/概念图，定量数据图仍需代码保证准确性
