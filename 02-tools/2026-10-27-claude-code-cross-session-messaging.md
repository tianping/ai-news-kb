# Claude Code 跨会话消息：让两个终端自己交接工作（克劳德猎手）

- **收藏日期**：2026-10-27
- **来源**：微信公众号「克劳德猎手」
- **原标题**：Claude Code 跨会话消息：让两个终端自己交接工作
- **分类**：工具与产品（Claude Code 功能）
- **URL**：https://mp.weixin.qq.com/s/_NudknXJcBF6LZS1SujovQ

## 概述

Claude Code 从 **v2.1.224** 起内置了跨会话消息：一个会话可以直接给另一个会话发一段话，对方收到后干活、再回话。这个功能默认开启，不用装插件，也不用配 MCP。它省掉的是「切到另一个终端，把结论复述一遍」这个动作。同时开的终端越多，越有用。

## 关键内容

### 1. 只传一段话
- 传的只是发送方 Claude 写的一段自然语言，通常几句话。**对话历史、改过的文件、截图都不会传过去。**
- 要把整段上下文搬过去，用 `claude --resume`。
- 设计目的：任务中途交接一个发现，比如「我改了 schema，你那边要 rebase」「迁移跑完了，可以开始测试」。通道刻意做窄，所以默认开着也不担心。
- 同机单条消息上限约 100 万字符，超过会在发送端被拒。

| 想做的事 | 用什么 |
|---|---|
| 让另一个会话知道一个结论 | 跨会话消息 |
| 把整段对话原样搬过去继续 | `claude --resume` |
| 让另一个会话接着自己干活 | 跨会话消息 + 对方自己的权限规则 |

### 2. 怎么用
- Claude 自己调用 `ListAgents` 找目标会话、调用 `SendMessage` 发消息。你只要说意图，比如「问 payments-api 迁移是不是跑完了」。
- `/list-agents`（简写 `/peers`）查看能联系到谁：当前会话的子代理、agent team 队友、本机其他会话（含后台会话）、云会话，以及通过 Remote Control 连着的其他机器上的会话。
- **v2.1.232+**：输入框里可以用 `@` 点名，例如 `@api-worker`，名字带空格写成 `@"release notes"`。云会话和其他机器上的会话，要先让 Claude 列过一次名单才能点名。
- **记得起名字**：启动时用 `claude --name migration`，运行中用 `/rename migration`。不起名会得到 `myapp-3f` 这样的默认名，同一目录开两个会话容易重名。
- 收到消息时会显示一行灰色预览 `> Message from @<发送者>: <第一行>`，按 `Ctrl+O` 或用 `--verbose` 启动可看全文。
- 接收方处理消息和用户手敲 prompt 一样计费、占上下文，**不要拿它做高频心跳**。

### 3. 最低版本

| 运行环境 | 最低版本 |
|---|---|
| macOS / Linux / WSL 2 | v2.1.224 |
| 原生 Windows | v2.1.234 |
| Bedrock / Google Cloud Agent Platform / Microsoft Foundry 等 provider 上的同机会话 | v2.1.248 |
| 跨机器主动发起对话 | v2.1.225 |

跨机器和云会话的发现，要求连着 Remote Control，并且会话用 claude.ai 账号登录。文章指出 claudefa.st 上的旧整理已有几条过时（说不支持 Windows、不支持 Bedrock、跨机器只能回复不能发起）。

### 4. 接收方怎么处理：`crossSessionInbound`

| 值 | 行为 |
|---|---|
| `accept` | 直接交给 Claude |
| `hold` | 先扣下，弹框问你是否放行 |
| `refuse` | 直接丢弃 |

- **不设时的默认规则**：接收方是普通权限模式（auto / acceptEdits / dontAsk）时，消息直接送达；接收方是 `bypassPermissions` 时默认扣下，只有发送方也在 bypass 模式才放行。在启用了 bypass 选项的交互终端里，Plan mode 也按 bypass 处理。
- 被扣下的消息会弹审批框，批准只放行这一条。审批框默认 5 分钟过期（`dialogExpiry`），每个会话最多暂存 100 条，超出时丢掉最旧的。
- 可以在 `/config` 里改「Messages from your other sessions」（v2.1.232+）。项目或本地设置里的 `refuse` 优先级最高。
- **`claude -p` 无人值守 worker**：没有界面弹审批框，被扣下的消息 5 分钟后过期丢弃。可以在 `--settings` 里设 `crossSessionInbound: "accept"`。bare 模式不绑定 socket，收不到消息，也不会出现在名单里。

### 5. 权限边界
- 接收方 Claude 会被明确告知：这条消息来自另一个会话，不是用户本人的指令。
- 消息**不能**替用户批准权限提示，也不能让 Claude 修改权限设置、CLAUDE.md 或其他配置。消息正文里的 `/compact` 之类命令只当纯文本，不会执行。
- 设置 `"isolatePeerMachines": true` 后，所有发往其他机器的消息都要你先批准，bypass 模式下也一样。任何一个设置作用域都能打开它，但项目配置关不掉它。
- 想完全关掉：不收消息就设 `crossSessionInbound: "refuse"`；不发消息、也不让别的会话看到自己，就在 deny 规则里写 `SendMessage` 和 `ListAgents`。注意 deny `SendMessage` 会连带切断发给子代理和 team 队友的消息。设成 refuse 的会话，在 `/status` 和名单里看不出区别。
- **传输路径**：本机会话之间走每个会话一个 Unix socket（原生 Windows 用命名管道），不经过 Anthropic 服务器；发往其他机器和云会话才走服务器。socket 只对当前操作系统用户开放，路径见 `/status` 的 Peer address，也通过环境变量 `CLAUDE_CODE_MESSAGING_SOCKET` 导出，认证用 `CLAUDE_CODE_MESSAGING_TOKEN`。
- **其他限制**：只支持纯文本；同一发送者的重复消息会被去重和限流；每个会话最多排队 50 条未读消息。两个会话互刷消息的循环会因此自动停下。

### 6. 四种常用搭法
1. **监控者 + 执行者**：监控会话发现问题后，不在自己这边修，发给 `claude --name worker-fix`，让 worker 在自己的 worktree 里改，改完回一条消息。监控会话的上下文一直保持干净。
2. **只给目标、不给上下文**：让另一个会话独立回答「你会选什么方案」，用来检验某个 skill 的引导是否足够清楚。可以同时开三四个会话跑同一个问题。
3. **等长任务报完工**：v2.1.236+ 的 `SendMessage` 有 `notify_when_idle` 参数，可以订阅另一个本机会话的空闲通知，收到完成时间和一行状态摘要。12 小时内没等到就自动取消订阅。
4. **跨机器对齐接口**（v2.1.225+）：两边都连着 Remote Control 时，用一条线程对齐 API 契约。坑：发送时自己没连 Remote Control，消息照样发得出去，但没有回复地址，对方回不了。

## 备注

- 版本号和设置项以官方文档为准，功能可能随版本变化。
- 本机环境（McCleary）的 Claude Code 里已经能看到 `ListAgents` 和 `SendMessage` 工具。
- 相关笔记：[Claude 上了 CarPlay，苹果只给它一张嘴](2026-10-30-claude-carplay-voice-mode.md)（同一公众号）
