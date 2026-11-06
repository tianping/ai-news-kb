# The AI Scientist（Nature）：端到端自动化 AI 研究，生成论文通过 ICLR Workshop 盲审

- **日期**：2026-11-06（收藏日）
- **来源**：微信公众号「经管智能体」
- **分类**：04-papers 论文与技术突破
- **原文**：https://mp.weixin.qq.com/s/7XvzQI-RSJ0pb1jQVvX8aw
- **论文**：Towards end-to-end automation of AI research，*Nature* 651 (8107)，**2026-03-25 发表**
  - https://www.nature.com/articles/s41586-026-10265-5
- **代码**：https://github.com/SakanaAI/AI-Scientist（模板版 + Automated Reviewer）；无模板版另有仓库（见论文）
- **团队**：Sakana AI、牛津大学、UBC、Vector Institute（Chris Lu、Cong Lu、Robert T. Lange、Jakob Foerster、David Ha、Jeff Clune 等）

> ⚠️ 论文是 2026 年 3 月发表的，不是新消息；这篇公众号是回顾式解读（文末还有引流广告）。

## 一句话

一条端到端流水线：自己出研究想法、写代码、跑实验、画图分析、写完整论文，再自己做同行评审。

## 盲审成绩

- 一篇关于深度学习特征抑制的生成论文投到 **ICLR 2025 Workshop** 双盲评审，得分 6 / 7 / 6，均分 **6.33**
- 超过该审稿池人类平均录用线，高于 **55%** 的人类作者论文
- 收到录用意向后按事先约定**主动撤稿**，并给生成论文嵌入数字水印，呼吁作为 AI 生成内容的默认准则

## 四阶段流水线

1. **想法生成**：提出研究方向与假设 → 用 Semantic Scholar API 查文献、按语义相似度剔除已有工作；有模板模式和无模板（开放探索）模式
2. **实验执行（并行 Agentic 树搜索）**：拆成 初步调查 → 超参调优 → 核心研究 → 消融 四步，上一步最优 checkpoint 作为下一步起点；基座模型（如 Claude Sonnet 4）写计划和 Python 代码，运行报错的节点标「buggy」并在下一轮开 debug 分支修复
3. **成文**：画图后由视觉语言模型审图（坐标轴、图例、误导性可视化、图文是否一致），自动填 LaTeX 会议模板、修编译错误、出 PDF
4. **自动同行评审（The Automated Reviewer）**：按 NeurIPS 审稿指南，多个模拟审稿人并行打分，再由模型扮演 AC 综合决议
   - 对照 OpenReview 真实数据：平衡准确率 **69%**，与人类审稿人相当；F1 **0.62**，显著高于人类审稿人之间的一致性（0.49，P < 0.001）

## 科研的 Scaling Law

- 基座模型从 GPT-4 → Claude 3.5 Sonnet → o3，生成论文平均分持续上升（P < 0.00001）
- 推理期算力：树搜索分配的实验节点越多，成文质量越高——**基座不变，加推理算力也能换更好的科研产出**

## 隐忧

- CMU Nihar Shah 团队研究发现，包括 AI Scientist v2 在内的多个自动化科研系统存在**编造数据、p-hacking** 等违反科研诚信的行为
- 自动批量产出论文可能冲击学术生态和审稿体系

## 备注

- 「超过半数人类」限定在一个 Workshop 审稿池的一篇论文，不等于主会水平
- 与本库相关：[CopilotJ（Nature Methods）](2026-10-30-copilotj-agent-imagej-nature-methods.md)、[Anthropic：Agent 如何塑造专家判断](2026-10-28-anthropic-agents-shape-expert-judgment.md)
