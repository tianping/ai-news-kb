# 让 Claude Code + Codex 连续重构 3 周：Skills 是假的、测试连上生产环境，一天烧 5 倍预算（CSDN）

- **日期**：2026-11-09
- **来源**：微信公众号「CSDN」（编译：苏宓），原演讲为 AI Engineer 频道 *I Let Agents Refactor My Codebase for 3 Weeks. Then I Read the Code*
- **分类**：02-tools 工具与产品（AI 编程实践/翻车复盘）
- **原文**：https://mp.weixin.qq.com/s/Y7O8QC2ipLURGP-n4ymECg
- **视频**：https://www.youtube.com/watch?v=6TjFjHPjVH0

## 概述

微软产品经理 Keiji Kanazawa（有开发和 AI 研究背景，工作中约 90% 时间用 GitHub Copilot CLI）让 Claude Code 和 Codex 轮流给个人项目写代码三周——一个写、一个审。表面上系统跑通了，直到一次摘要出错去查 trace，才发现架构是“名字对、实质错”，最后推倒重来。

## 项目

播客下载→转录→摘要 → 加入 X/Threads 抓取、24 小时调度、邮件收件箱（发链接回摘要）→ 演变成 Karpathy 说的「LLM Wiki」→ 再想做真正的 Agent Runtime：Agent 注册中心、独立调度、对接多家 Agent SDK，外加深度研究、每日反思（分析自己的 Claude Code 使用记录）、自动沉淀技能。

## 翻车清单

| 问题 | 实际情况 |
|------|---------|
| Agent SDK | 根本没用；问 trace 时 Claude Code 答“这不是真正的 Agent” |
| Agent Skills | 没有独立 skill.md，也没有底层脚本 |
| Actions | 和 Skills 同一套逻辑，只换了名字（代码里 `action` 直接被当成 `skill`） |
| 每日反思 Agent | 定时照跑，但重构弄坏了 Prompt 逻辑，产出全是空的 |
| 单元测试 | Codex 把测试接到**生产推理接口**，一天跑 30 多轮，花约 100 美元，约平时一周预算的 5 倍 |

作者原话：“它们给这些构造起了听起来很合理的名字，但实际上根本不是它们所声称的那样。”而且 Spec 和实施计划他都审过、批过——问题出在只看计划、不看实现。

## 重来后的做法

1. **工具选型**：对比 Claude Code、Codex、GitHub Copilot CLI/应用、Mario Zechner 的 Pi。个人项目用 Claude Code + Codex，工作仍用 Copilot。Pi 做交互快又漂亮，但一上来问 SQLite 表结构、TypeScript API 怎么设计——偏工程师，不适合产品经理
2. **Spec + Plan 之外加 Rubrics（评分标准）**：Agent 在他引导下生成 Rubrics；生成 Agent 出结果 → 评估 Agent 多维打分 → 不达标就提取失败原因、自动改 Prompt 和 Schema → 再生成，爬山式迭代
3. **交叉审查**：Claude Code 和 Codex 互审对方代码，多轮讨论和修复

## 案例：深度视频分析 Agent

对象：Dwarkesh Patel 播客里 Reiner Pope 的板书讲座（大模型在 GPU 集群上训练/推理的数学，Roofline 分析）。

流程：截 440 张关键帧 → 用 eval 筛出信息量最高的板书 → OCR 公式 → 关联 arXiv 原文 → 按章节放进 Canvas，可高亮公式继续追问。

四项 Rubrics：
- **机制解释**：讲透因果机制，而不只是提概念名
- **证据可信**：每个核心结论都有依据
- **内容覆盖**：保留视频中的高价值增量信息
- **深入延展**：给想深挖的人指明后续路径

**Agent 会对评分标准过拟合**：Rubrics 围绕这一段视频优化，这段越来越好，换 5 段别的视频就不行。之后把多类视频纳入测试、调整 Rubrics，才稳定下来。结论：高分≠完成任务，人要检查 Rubrics 本身能否泛化。

## 读不读代码：L vs Z

- **L 派**（AI Engineer Europe 上 OpenAI 演讲者）：读代码是负担，不如砸 Token 让 AI 多干
- **Z 派**（Mario Zechner）：要读代码、放慢节奏
- 作者仍站 L：“我负责目标……大部分时候我仍然不会读代码。”靠目标 + Rubrics + 交叉审查约束 AI
- 金句：“Token-maxing is what makes a product manager an AI engineer.”

## 启示

- 审批 Spec/Plan 不等于验收实现；关键抽象（SDK、Skill、Agent）要有可检查的证据（trace、文件、调用记录）
- 测试默认不能碰生产推理接口；给 Token 账户设预算告警
- 定时任务要监控**输出内容**，不只看“跑了没有”
- 评分标准同样会被优化钻空子，要用多样本验证泛化
- 与 [Claude 开发 + GPT 评审](2026-11-06-claude-builds-gpt-reviews-workflow.md) 思路一致：开发和评审分离、不通过就返工
