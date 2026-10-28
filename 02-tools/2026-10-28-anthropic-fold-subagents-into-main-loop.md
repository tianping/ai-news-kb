# Anthropic：把 subagent 并回主 Agent 循环（单 Agent + Skills + Tool Search）

- **收藏日期**：2026-10-28
- **来源**：微信公众号「cxuanAI」
- **原标题**：别学 subagent 了，已经要凉了。
- **原始文章**：Anthropic《Building agents: When to fold subagents into your main agent loop》
- **分类**：工具与产品（Agent 架构 / 最佳实践）
- **URL**：https://mp.weixin.qq.com/s/UZ8vdHayvROiXRiqitbeAQ

## 概述

Anthropic 应用 AI 团队对比了多个企业级部署，结论是：**一个带多个 Skills 的单 Agent 主循环，质量往往优于「万能提示词」和多 subagent 方案，单任务成本和延迟也更低**。他们建议用 Skills 和 Tool Search 按需提供上下文与工具，取代按部门拆分的 subagent。原文同时推了一个迁移工具包，可自动把 subagent 并入主循环。

> 公众号标题「subagent 要凉了」夸张了。原文并没有否定 subagent，而是指出它被滥用了。

## 为什么多 Agent 架构常常失效

- **康威定律**：企业按部门建客服 Agent、物流 Agent、退款 Agent，再在上面放一个编排器，等于把组织架构照搬成了 AI 架构。
- **传话游戏（telephone game）**：信息每经过一次 subagent 转述和摘要，就丢一次细节，延迟和 token 成本却翻倍。
- **五类企业失效模式**：
  1. 上下文管理难，交换时容易丢细节
  2. subagent 泛滥，累积几百个功能重叠的，出事找不到负责人
  3. 拼凑式 prompt，每个团队往 system prompt 里加一句，规则互相冲突
  4. 局部评估，各自为自己的 subagent 过度优化，没有统一负责人
  5. 延迟高，客户实测单 Agent 主循环约**快 2 倍**
- 常见的过度设计：意图分类器、固定多步推理流程、用户分群逻辑、独立推荐分支，再加多 Agent 协调器。实际上多数场景只需要**一个清晰的 prompt、4–5 个轻量工具和稳健的 guardrails**。

## subagent 仍然适用的场景

- 单 Agent 循环中上下文膨胀，导致性能明显下降
- 需要并行处理、做完即可丢弃的任务
- 单 Agent 难以选对工具，或 system prompt 里有互相冲突的指令

要用多 Agent 时，从简单模式起步：generator → verifier，或 orchestrator → subagent。**按上下文边界拆分，不要按问题类型拆**，每个 subagent 只返回精简结果。

## 渐进式披露：Skills + Tool Search

- 起步时 Agent 只看到 Skills 和工具的简短描述，真正需要时才加载完整内容。
- **Tool Search 和 Tool Use 的区别**：Tool Use 是调用工具；Tool Search 解决的是先发现、再加载。模型先搜工具目录，命中后才把那个工具的完整定义载入上下文。原文提到，system prompt 里暴露的工具超过 **30–50 个**后，工具选择会变差。注意：用官方 API 时，客户端仍要提交全部工具定义，延迟加载控制的只是这些定义何时进入模型上下文。
- 这样做的三个依据：
  1. **共享上下文**：长任务的每一步结果都留在主上下文里，后面的步骤能接得上。
  2. **渐进式披露**消除了「上下文放不下所以要拆 subagent」这个主要理由。Claude Code 针对更强模型的系统提示词缩减了 80% 以上。
  3. **模型越来越强**：从 Opus 4.5 起，单次循环能处理的任务量超出多数团队预期。Opus 5.5、Fable 5.1 的思考过程不能关闭，会主动汇报进展、自行核对结果，harness 里的部分组件变得多余，更多决策会交给模型本身。

## 单循环设计蓝图

- **运行机制**：一个模型维护一个全局上下文。第一轮先写执行计划，之后每轮批量并行调用工具，harness 并发执行并返回原始数据。模型看到结果后可以**边执行边改计划**，再自己决定是继续调用工具、追问用户，还是给出结论。循环在给出最终回答或触达预算（轮数、token）时终止，必要时外挂一个轻量 verifier。
- **职责四分法**：
  | 组件 | 职责 |
  |---|---|
  | 模型 | 全局决策与动态规划，外层写死的重新规划逻辑可以删掉 |
  | Skills | 原各 subagent prompt 里的业务 SOP，按需加载 |
  | Tools | 读写操作、检索；**硬规则和安全防线（校验、审批、权限）放进 Tool 内部**，缺参数或越权就返回明确提示 |
  | 传统代码 | 低开销的确定性工作，比如正则提取订单号注入上下文、构造 payload、解析流式响应 |
- 单 Agent 也需要严格把控：精心管理 system prompt，让 Skills 和工具描述在合适时机触发，审慎处理上下文、记忆和笔记。建议由**一个核心团队**负责 system prompt 的端到端质量，新工具和评估上线前都要经过审核。

## 迁移步骤

1. **先建评估集**：核心团队维护共享的端到端测评，各 Skill 团队随自己的 Skill 发布小型评估集。有 orchestrator 时还要评估路由准确率。
2. **逐步减少 subagent**，三种方式可分阶段采用：
   - **合并领域 Agent**：相互依赖、耦合紧密的 Agent 先合并成一个领域 Agent。
   - **subagent 作为工具**：由主 Agent 掌控对话，只有确实需要与用户交互时才让 subagent 接管。代价仍在：返回的是有损摘要，嵌套循环也会增加延迟。
   - **改写为 Skills + MCP 工具**：prompt 转成 Skill，API 调用转成 MCP 工具，对比有无该 subagent 时的表现，决定是否保留。
3. **看翻转，不只看总分**：要检查具体哪些任务的结果变了。案例：客服 Agent 合并后延迟下降了，但主 Agent 查订单时没有向物流 subagent 询问配送日期，直接给出了答案，原本能顺利完成的流程坏了。

## 多团队如何共建一个 Agent

- 所有权从「Agent 层面」下放到「Skills 和工具层面」：
  - **MCP 工具**：执行操作、读取实时数据、按固定格式返回结果
  - **Skills**：团队的做事步骤、规则和判断标准
  - **知识库**：参考资料，通过搜索工具获取或放进 Skill 的参考文件。**不要用代码检索后粘贴进 prompt**。
- Skill 库维护交还给各自的创建者：用 `/skill-creator` 创建和审计（精简、去重、重构），用 `/skill-doctor` 查看各 Skill 的成本和使用频率，清理冗余。
- 一句话：Tools 管执行，Skills 管方法，知识库管资料，核心团队管最终有没有把事做成。

## 备注

- 和 OpenClaw、Claude Code 的用法直接相关：本机的 skill 体系（openclaw-skills、wechat-kb-notes 等）就是按需加载的 Skill 模式。只有上下文隔离或需要并行时才值得开 subagent。
- 细节以 Anthropic 原文为准，公众号的转述有一定解读成分。
