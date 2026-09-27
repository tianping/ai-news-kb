# 想搭建 Agent，架构怎么选？——9 种主流 Agent 架构全解（从轻到重）

- **日期**：2026-09-27
- **来源**：组织AI洞察局（微信公众号）
- **分类**：AI Agent / 架构方法论
- **URL**：https://mp.weixin.qq.com/s/aPr1jbM0MB21iKgfu3pIFg

## 概述

Agent 落地时不止"单 Agent vs 多 Agent"两条路。文章从轻到重梳理了 9 种主流架构，每种给出优缺点 + 适用场景。核心结论：架构互不互斥，生产系统往往是拼出来的（外层 Graph Workflow 编排、内层节点跑 ReAct、最前叠一层 Router+Skill 做意图分发）。选架构就看两件事：**场景有多复杂 + 需要多强的控制力**。

## 关键内容

**9 种架构速查表**

| # | 架构 | 一句话 | 优点 | 缺点 | 适用 |
|---|------|--------|------|------|------|
| 1 | 单一 Agent | 一个大模型包揽全部 | 简单、低延迟、低成本 | 复杂任务认知过载、上下文污染 | 原型验证、Demo、简单问答 |
| 2 | Pipeline 流水线 | 多 Agent 固定顺序接力 | 易实现、易调试、可审计 | 不灵活、一环节错全链断 | 可拆解为顺序步骤、需可预测/可审计 |
| 3 | P2P 对等协作 | 去中心化圆桌协商 | 鲁棒、信息自由流动 | 不收敛、重复劳动、协调成本高 | 多角色深度讨论、群体智慧 |
| 4 | ReAct | 思考→行动→观察循环 | 多步探索、可解释、支持工具调用 | token 消耗大、易跑偏死循环 | 检索+推理结合的探索型任务 |
| 5 | Plan-and-Execute | Planner 一次出计划，Executor 逐步落地 | 稳定、解耦、计划可复用 | 计划错则全盘崩、缺纠偏 | 代码生成、长流程自动化 |
| 6 | 多 Agent 协作 | 协调器统筹 Planner/Reviewer/Executor | 职责隔离、可扩展、质量把关 | 成本高、协调器是关键路径 | 金融/医疗合规等强一致性场景 |
| 7 | **Router + Skill** | 意图路由→固定 Skill 执行 | 稳定性极强、可灰度回滚、可缓存 | Skill 设计成本高、长尾覆盖差 | **AI Coding/智能体系统（作者当前主力落地）** |
| 8 | Blackboard 黑板系统 | 共享状态驱动、事件触发 | 松耦合、支持并行 | 状态管理重、因果链难追 | 多专家联合分析、事件型系统 |
| 9 | Graph Workflow | DAG 显式编排（LangGraph/Temporal/n8n/Prefect） | 企业级稳定、可回溯重试、可观测 | 最重、建模运维成本高、过度设计风险 | 严格 SLA 的关键业务链路、数据管道 |

**作者最强调的**：第 7 种 Router + Skill 是"目前落地业务时用的最多的一种"——核心是**不要让模型自由发挥，让模型做选择**（意图识别→路由到封装好的 Skill）。AI Coding 方向是工程界验证过的最优实践之一。

**组合使用原则**：外层 Graph 编排 → 内层节点 ReAct 探索 → 最前 Router+Skill 分发，三层叠加是常见生产模式。

## 使用判断（什么时候用哪种）

- **快速验证 / 简单问答** → 单一 Agent 或 Pipeline
- **需要可解释 + 工具调用 + 多步探索** → ReAct
- **步骤可预先确定的长流程** → Plan-and-Execute 或 Graph Workflow
- **意图明确可枚举 + 要稳定可控** → Router + Skill（AI Coding / 客服 / 代码助手首选）
- **强合规多角色** → 多 Agent 协作（协调器 + Reviewer）
- **多子任务共享状态** → Blackboard
- **生产级 SLA / 可回溯重试** → Graph Workflow

## 影响/备注

- 与 ai-news-kb 已收录的「Claude Code 最强配置」互补：那篇讲单 Agent 内部的 CLAUDE.md/Rules/Skills/Hooks 多层控制；这篇站在更上层讲"选哪种 Agent 拓扑"
- Router + Skill 的思路和 Claude Code 的 `.claude/skills/` 目录（按需加载多步骤流程）异曲同工
- 代表工具提及：LangGraph、Temporal、n8n、Prefect
