# Claude Code 有 4 种 Agent 模式，多数人只用了一种（AI 效能加速）

- **收藏日期**：2026-11-19
- **发布日期**：2026-11-15
- **来源**：微信公众号「AI 效能加速」（卓行信息科技）
- **原标题**：Claude Code 有 4 种 Agent 模式，多数人只用了一种
- **分类**：工具与产品（Claude Code 使用方法）
- **URL**：https://mp.weixin.qq.com/s/NDuewv6sbohrxqV_5_mfvQ
- **主要依据**：Kristopher Dunham 的 Medium 长文《Claude Code Has Four Agent Modes. Most Developers Only Use One.》+ Anthropic 官方文档

## 概述

大多数人一直把 Claude Code 当单会话聊天框用。任务一复杂（比如七八个文件的重构），大约 20 分钟后它就会忘掉刚定好的方案、重复读同一个文件。原因不是模型变笨，而是所有活都塞进了同一个上下文窗口。文章的主张是：按任务复杂度在 4 种模式之间切换，把中间过程拆出主窗口。

| 模式 | 结构 | 适合 |
|---|---|---|
| 01 单会话交互 | 一个终端、一个上下文 | 一句话说得清、你能盯住的小改动 |
| 02 Subagent 子代理 | 辐射状委派，独立窗口，只回传摘要 | 跑测试、翻日志、全库搜索 |
| 03 Agent Teams | 共享任务清单 + 成员间直接发消息 | 前后端+测试这类需要边改边对齐的并行任务 |
| 04 Routines 云端例程 | 定时 / API Webhook / GitHub 事件触发 | 你不在电脑前也要跑的低风险杂活 |

## 关键内容

### 1. 单会话交互
- 主代理用 Bash、Edit、Grep、LSP 读代码、改文件、跑命令。你在旁边看着、随时打断时最顺手。
- 毛病是每次读 500 行文件、跑一轮报错测试都堆进同一个窗口，历史噪音越积越多。
- 原文的说法：单会话只适合「你一眼能看住」的活。

### 2. Subagent 子代理
- 思路：「你去查清楚，把干净的结论带回来，别把过程塞进主对话。」子代理在独立上下文里干活，只回传浓缩结果。
- 内置 **Explore**（快速只读检索）和 **Plan**（规划前只读调研）。
- 自定义：在 `.claude/agents/` 下放一个 Markdown 文件：

```markdown
---
name: test-runner
description: 运行测试并汇总失败用例。需要跑测试时调用，不要在主会话刷日志。
tools: Read, Bash, Grep
model: haiku
---
只负责运行测试并提炼出错文件、行号与核心堆栈，不要修改业务代码。
```

- 三条工程原则：
  - **description 当 API 契约写**：主代理靠它判断何时派活。写清触发条件，比写「你是资深专家」有用。
  - **tools 白名单，给最小权限**：做代码审查只给 `Read, Grep, Glob`；需要隔离修改可加 `isolation: worktree`。
  - **model 按活分配**：翻测试日志用 haiku，又快又省；深度推理才用 sonnet/opus。
- 局限：辐射状（Hub-and-Spoke）结构，子代理之间不能互相通信。子任务需要边改边对齐时，就得用 Agent Teams。

### 3. Agent Teams 团队
- 开启实验特性 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 后，主会话变成 Team Lead，拉起一组各有独立窗口的 Teammate。
- 成员共享一份带依赖锁的任务清单，通过本地邮箱 `~/.claude/teams/{team-name}/inboxes/` 点对点发消息。比如后端要改接口契约，可以直接通知前端，不用经主会话转达。配合 tmux/iTerm2 可以分屏看它们并行干活。
- 官方推荐的两种用法：
  - 多视角并行审 PR（安全、性能、测试覆盖各管一块）
  - 用竞争假设排查诡异 Bug：派 3～5 个成员各自举证、互相证伪
- 代价：每个 Teammate 都是独立实例，Token 消耗随人数线性增长。只有等待的成本高过 Token 账单时才值得开团队。

### 4. Routines 云端例程
- 2026 年 4 月推出（新版文档里也叫 Scheduled Tasks）。把 Prompt、GitHub 仓库和外部连接器（Slack、Linear 等）打包放到云端虚拟机里跑。
- 三种触发方式：
  - **Schedule**：按 Cron 定时，最短间隔 1 小时。例如每周日凌晨扫描过期依赖并提 PR。
  - **API Webhook**：生成专属 HTTP 端点，可接 Sentry/Datadog。半夜线上报警时，它翻堆栈、定位 commit、开好修复草稿 PR，值班工程师早上直接 review。
  - **GitHub 事件**：例如 `pull_request.opened` 时自动审查，发版时自动汇总 Release Notes。
- 创建入口：`claude.ai/code/routines`，或在终端输入 `/schedule`。
- 防失控措施：云端默认只允许 Trusted 网络白名单（越界返回 403），只能推送到 `claude/` 前缀的分支，碰不到 main。

### 5. 权限模式是另一套开关，别和 Agent 模式混淆
- Shift+Tab 切换的 Plan、Auto 属于**权限模式**（Permission Modes），和 4 种 Agent 模式是两个独立维度：Agent 模式决定「派几个代理、在哪跑」，权限模式决定「每一步要不要你点头」。
- 常见用法：先切 Plan 只读出方案，定稿后切 Auto 放手干。
- 从 **v2.1.283** 起，由分类器模型把关的 Auto 模式是终端默认起点。
- 相关数据（均为原文引用）：
  - 手动模式下，用户不看就直接批准的权限弹窗约占 **93%**（Backslash Security）
  - 对危险指令的拦截率：Auto 模式分类器 **89%**，人工 **13.6%**
  - 分类器漏报率约 **17%**
  - Anthropic 实证研究覆盖 **40 万+** Claude Code 会话；委派任务中约 **27%** 是「没有 AI 原本根本不会做」的新增工作
- 两条硬边界：
  - 遇到外部 Sentry 堆栈、Fork PR 描述这类内容，存在 Prompt 注入风险，需要用 deny → ask → allow 硬规则兜底。
  - Routine 显示运行成功，只说明容器正常退出，不说明代码没幻觉。Diff 一定要自己看。

### 6. 升级路线（三步）
1. **先写 1 个 Subagent**：挑最占主窗口的脏活（跑测试、翻日志），写十行配置，用顺手了再加第二个。
2. **真遇到跨模块的难题，再试 1 次 Agent Team**：比如前后端+测试并行，盯着 Token 账单，看值不值。
3. **挑一个低风险的每周杂活，跑第 1 个 Routine**：用 `/schedule` 做每周依赖检查或死代码清理，只给 `claude/` 分支权限。

结论：写代码的单次成本足够低以后，真正值钱的是三件事：把任务定义清楚、选对委派模式、像审人写的代码一样认真看每个 Diff。

## 个人备注

- 和 [Anthropic：把 subagent 并回主 Agent 循环](2026-10-28-anthropic-fold-subagents-into-main-loop.md) 的观点有张力：Anthropic 企业部署发现「单 Agent + Skills」常优于多 subagent。可以这样理解：subagent 适合隔离噪音大、边界清晰的只读任务，不适合拆分需要共享上下文的推理。
- Agent Teams 成员间通信，可对照 [Claude Code 跨会话消息](2026-10-27-claude-code-cross-session-messaging.md)。
- 文中百分比数据多为二手引用（安全厂商博客、媒体报道），引用前最好回查原始出处。

## 参考资料（原文所列）

1. Kristopher Dunham：Claude Code Has Four Agent Modes. Most Developers Only Use One.（Medium）
2. Anthropic 官方文档：Create custom subagents（code.claude.com/docs/en/sub-agents）
3. Anthropic 官方文档：Orchestrate teams of Claude Code sessions（code.claude.com/docs/en/agent-teams）
4. Kristopher Dunham：Claude Code Routines Just Turned Your AI Into a 24/7 Developer（Medium）
5. Anthropic 官方文档：Routines / Scheduled tasks（code.claude.com/docs/en/routines）
6. Anthropic 官方文档：Choose a permission mode（code.claude.com/docs/en/permission-modes）
7. Backslash Security：Review Claude Code Auto Mode Risks
8. itdoeswhatnow.com：Auto Mode 默认开启报道
9. Anthropic 研究：Agentic coding and persistent returns to expertise
