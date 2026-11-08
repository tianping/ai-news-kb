# 科研绘图只留 3 个 Skill：scientific-visualization、nature-figure、paper-illustration 实测（学术AI大模型）

- **日期**：2026-11-08（收藏）
- **来源**：微信公众号「学术AI大模型」（作者：全息学术）
- **原标题**：GPT 6 的绘图skill那么多，我只保留了博导推荐的这3个
- **分类**：06-academic（科研绘图）
- **URL**：https://mp.weixin.qq.com/s/PLMN42FaNPrEFn1-hdpLVg
- **相关笔记**：`02-tools/2026-08-10-research-figure-skills-top10.md`（科研绘图 Skill Top 10，含 nature-figure、scientific-visualization 所在仓库）、`02-tools/2026-08-16-99硕博生科研绘图-codex-skill.md`、`06-academic/2026-10-20-research-skills-selection-guide-11-projects.md`
- **本篇新增**：三个 Skill 按用途分工 + 三个实测案例（热力图、回归系数图、研究框架图）

## 三个 Skill 怎么选

| Skill | 适合 | 输出 | 地址 |
|-------|------|------|------|
| **scientific-visualization** | 已有实验数据，要画好比较的结果图：选图形、坐标、配色，检查误差、缺失值和图例；单图、多组比较、多面板都行 | 图 + SVG + 绘图代码 | [K-Dense-AI/scientific-agent-skills](https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/scientific-visualization) |
| **nature-figure**（nature-skills 合集的画图模块） | 围绕论文要表达的结果设计配图，把几组结果组织成多面板图，统一坐标、字体和视觉层次；也能改已有图 | 画前问用 Python 还是 R；保留 SVG | [Yuan1z0825/nature-skills](https://github.com/Yuan1z0825/nature-skills/tree/main/skills/nature-figure) |
| **paper-illustration** | 研究框架、方法步骤、模块关系的示意图；给方法描述或草图，它整理内容、生成示意图并检查模块与连线 | PNG（逐个改文字箭头要重画） | [wanshuiyin/Auto-claude-code-research-in-sleep](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/paper-illustration) |

安装：把项目地址发给 Codex，说「请从这个地址安装 xxx」。

## 实测

**1. 热力图（scientific-visualization）**
- 材料：Rafigh 等人的供应链论文，比较三种算法在多个算例上的求解结果
- 做法：每个算例以三种算法中最好的结果为基准，格子里写其他结果比基准高出的百分比
- 效果：不同量级的原始值转成同一算例内的相对差距后，颜色负责快速比较，数字保留细节；例外一眼可见（P7 颜色明显变深，说明混合算法这一例落后；P8 也非最优但差距小，仍是浅色）
- 提示词：「请使用 scientific-visualization，把论文表格中的算法结果整理成热力图，比较每个算例中各算法与最好结果的差距。」

**2. 回归系数图（nature-figure）**
- 材料：《管理世界》一篇论文的投资、消费两张基准回归表
- 做法：双面板系数图，圆点为系数，横线为「系数 ± 1.96 × 聚类稳健标准误」
- 效果：原本分散在两张表里的四个模型估计放进同一视野；两面板横轴范围一致，便于比较
- Python 和 R 各画一次，质量差别不大，用自己熟悉的即可——图好不好更取决于数据和版面设计
- 提示词：「请使用 nature-figure，根据论文两张基准回归表绘制双面板系数图，并展示估计区间。」

**3. 研究框架图（paper-illustration）**
- 材料：同一篇供应链论文的供应商选择、订货库存决策、中断风险、可持续目标
- 效果：供应商、采购决策、制造环节层级清楚，风险与可持续目标用不同颜色区分
- 原项目用 Gemini 跑流程，作者沿用其设计与审查思路改用 GPT 图像模型生成
- 提示词：「请使用 paper-illustration，根据论文中的供应商、采购决策和中断风险绘制研究框架图。」

## 结论

三个 Skill 功能有重合，先看手里有什么材料、想让读者从图里看到什么：数据结果比较 → scientific-visualization；多组结果组成论文配图 → nature-figure；方法与关系示意图 → paper-illustration。

## 备注

- 标题里的「博导推荐」原文没有交代是哪位博导
- 示意图是生图模型出的 PNG，文字和数值要人工核对；数据图走代码生成，更可复现
- 正文由 fetch 脚本抓取，图片未收录
