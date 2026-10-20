# Supervisor-Skills：港科大（广州）骆昱宇把博导科研经验做成 11 个 AI 技能

- **日期**：2026-10-20（收藏）
- **来源**：微信公众号「学知大模型」
- **原标题**：太好用了！Supervisor-Skills把顶级博导的科研经验蒸馏成AI技能！
- **分类**：06-academic（AI 学术应用 / 科研 Skill）
- **URL**：https://mp.weixin.qq.com/s/EwQ1g1Ldk3_9yODoMcWhAg
- **项目**：https://github.com/HKUSTDial/Supervisor-Skills
- **相关笔记**：[GitHub Star Top 10 科研学术 Skill（8 月）](2026-08-11-github-top10-research-skills.md)（当时第 8 名，⭐5,351）、[9 月科研 AI Star 榜](2026-09-07-github-research-ai-stars-sept.md)

## 概述

- **作者**：骆昱宇，港科大（广州）数据科学与人工智能学域助理教授、博导，数据智能与分析实验室负责人；清华计算机博士，福布斯中国 30 Under 30；在 SIGMOD、VLDB、ICML、NeurIPS 等发表论文 50 余篇。
- **做法**：把十年在数据科学/AI 顶会发表和审稿积累的经验与「学术直觉」，封装成大模型能按流程执行的技能。结构是 **Handbook**（方法论：怎么做科研、组织论文、判断想法）＋ **Skills**（拆成具体任务按固定流程执行）。
- **热度**：发布 32 天破 1,000 星，现在超过 **7.4k**；已和豆包合作，作为**豆包·办公模式**的核心科研技能，驱动「科研评估」和「学术创作」两项官方能力。
- **要解决的「最后一公里」**：读了很多科研指南还是不会写；导师没空逐个评估想法；投稿前不确定逻辑、图表、低级错误；大模型强，但缺判断力和学术品味。

## 11 个 Skill（四条工作线：选题调研 / 结构与写作 / 表达与作图 / 投稿与审稿回应）

**建议先用的 4 个**：

| Skill | 做什么 |
|---|---|
| **idea-evaluator** | 用五个维度评估研究想法：Higher（创新）、Faster（效率）、Stronger（鲁棒）、Cheaper（成本）、Broader（泛化）。同时看想法处在什么生命周期阶段、和研究者能力是否匹配、有没有致命问题，给出 Strong Accept / Accept with Revisions / Reject and Pivot |
| **deep-research** | 多视角（对抗性）检索 → 引用核验 → MECE 分类 → 句内交叉比较 → 综合，围绕研究问题组织成一份 survey，而不是一堆摘要 |
| **intro-drafter** | 六段式 Introduction：背景与贯穿案例 → 已有研究的不足 → 问题本质与目标 → 关键挑战 → 方案概览 → 贡献。贡献与挑战一一对齐，引用经过真实检索核验。适合搭框架，逻辑顺了再润色 |
| **pre-submission-reviewer** | 投稿前从五个维度挑硬伤：宏观逻辑、写作细节、英语语法、LaTeX 格式、图表质量；按 CRITICAL / MAJOR / MINOR 分级；会标出 AI 腔词汇和 em-dash 滥用 |

**其余 7 个**：
- **tech-paper-template**：动笔前锁定论文逻辑骨架，做四点自洽性检查；
- **paper-writer**：证据门控写作，每个论断都能追溯到作者材料或经过验证的检索；
- **paper-polish**：忠于原意的润色，去 AI 腔，支持中译英；
- **figure-designer**：规划三张承重图（Motivated Example、Solution Overview、Experiment）；
- **drawio-reconstruction**：参考图、论文图、截图重建为可编辑的 Draw.io 文件；
- **benchmark-paper-template**：Benchmark 论文的五支柱审计与投稿前检查清单；
- **vibe-research-workflow**：Vibe Coding、Vibe Figure、Vibe Writing 的 AI 辅助科研流程与行为规则。

这些 Skill 对应两条完整的论文生命周期：技术论文和 Benchmark 论文。

## 安装

把下面这段话发给 AI 助手（每个含 `SKILL.md` 的目录单独装成一个 skill，不要把仓库根目录当成一个 skill）：

```
Help me install every skill under the skills/ directory from
https://github.com/HKUSTDial/Supervisor-Skills.
Treat each directory containing SKILL.md as a separate skill;
do not install the repository root as one skill.
If you are Codex, use $skill-installer.
```

装好后靠自然语言自动触发。

## 备注

- 和本机已装的 **academic-research-skills（ARS）** 插件功能重叠：ARS 也有 deep-research、论文写作、审稿模拟、引用核验。Supervisor-Skills 的特色是 idea-evaluator（五维 + 生命周期）、六段式 intro、投稿前分级挑错、Draw.io 重建，面向数据科学/AI 顶会。生信论文想试的话，可以挑 idea-evaluator、pre-submission-reviewer、drawio-reconstruction 单独装，避免和 ARS 的 deep-research 同名冲突。
- 原文末尾有「后台回复关键词领 150+ 科研 Skill 表格」的公众号引流，已略去。
- 「亿级用户」「7.4k Stars」为原文数字，未核实。
- 许可证：**CC BY-NC-SA 4.0**（非商业、相同方式共享），据同日 [科研 Skill 选型指南](2026-10-20-research-skills-selection-guide-11-projects.md) 的核对。
