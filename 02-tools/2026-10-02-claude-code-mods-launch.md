# 刚刚，Claude Code 推出 Mods 功能，一句话魔改自己

> 收藏自微信公众号 · 2026-10-02 · https://mp.weixin.qq.com/s/guoG9O02Ryza01Im2cVTVw
> 官方博客：https://claude.com/blog/claude-code-mods

## 核心内容

**Claude Code 2.1.287 正式上线 Mods（Claude Mods）**：用几行 TypeScript 改掉 Claude Code 的行为、重画界面、把内置功能换成自己写的版本。不会写也没关系，直接跟 Claude 说，它自己写、装、热重载，当前会话即刻生效。

### Mods 是什么

- **事件驱动中间件**：Claude Code 每做一件事都发事件（工具调用、权限请求、prompt 提交、对话轮次开始/结束、UI 渲染等）
- 一个 Mod = 挂在某个事件上的函数：可在事件前跑、后跑、接管（不调 next 自己返回结果）、或包裹前后各跑一段
- 三参数签名：`on("event", {matcher}, async ($, e, next) => {})`
  - `$` = Mods API（ui、session、state、store、fs、process、http、tool、command、model…）
  - `e` = 事件输入（纯数据）
  - `next` = 传给下一个插件/内置行为
- 三种动作：**观察**（先 next 再看结果）、**改写**（改 e 再 next）、**接管**（不调 next，自己返回）
- 多 Mod 同事件按加载顺序执行，先加载的最外层（洋葱式嵌套）

### 与旧 hooks 区别

| 能力 | Settings hooks | Mods |
|------|----------------|------|
| 改写事件 | ❌ | ✅ |
| 画新界面/替换 UI | ❌ | ✅ |
| 替换内置功能 | ❌ | ✅ |
| 状态保持 | ❌（每次跑 shell） | ✅（常驻会话） |
| 调用 Claude Code API | ❌ | ✅（开面板、跑进程、注册命令、给模型注册工具） |

### 三个官方示例 Mod

1. **Token Weather**（~80 行 / 可一句话生成）：输入框上方实时显示上下文「天气预报」
   - 25% 以下 ☀、25-49% ☁、50-74% ☂、75-89% ☇、90%+ ↯
   - 显示已用 %、token/窗口、最近 12 轮迷你柱状图、上轮增量
   - 桌面端/终端同步显示

2. **Blast Radius**：危险命令干预面板
   - 扣下 `rm -rf`、`git reset --hard`、`git clean`、强推、DB 迁移等
   - `git status --porcelain`/`git clean -n` dry-run 列出受影响文件
   - Proceed / Cancel 面板，取消则返回 deny 理由给模型
   - ⚠️ 只是安全网非权限系统：命令文本匹配，`$(…)`、别名、脚本可绕过

3. **Replay Theater**：编辑回放
   - 记录每次 Edit/Write 的文件、改前改后 diff
   - 一轮结束后输入框上方出现回放提示，`r` 或 `/replay` 打开面板逐步回放
   - 纯观察不拦截；全屏靠右、窄屏内嵌输入框上方

### 让 Claude 帮你写 Mod

- 交互式会话里直接描述（如「做个 mod 在输入框上方显示当前 git 分支」）
- 内置 `plugin-authoring skill`，或手动 `/plugin-authoring` 加载
- 演示：「把工具输出里的高熵密钥抹掉，并告诉模型抹了几个」→ 自动加载 skill、写清单、写 hook、跑校验
- 另一个演示：「把屏幕上的数字和邮箱都藏起来，鼠标悬停再显示」→ 挂 `ui.render`，Claude 读到真实文本、只变显示

### Mod 生命周期与分发

1. 写入 `~/.claude/dev-mods/<会话ID>/`（受保护路径，需批准每个文件）
2. 保存第一个文件时询问是否开启热重载（本会话即时生效，后续改动轮次结束时重载）
3. `/plugin` → Installed 标签页查看/开关
4. 想留存：拷到 `~/mods/xxx`，用 `claude --plugin-dir ~/mods/xxx` 启动，或发布到 marketplace
5. marketplace：带 `.claude-plugin/marketplace.json` 的 GitHub 仓库
   - 安装：`/plugin marketplace add org/repo` → `/plugin install name@marketplace` → `/reload-plugins`
   - 也可 `claude plugin install <name>@<marketplace>`
- 验证/测试：`claude plugin validate ./mod` / `claude plugin test`

### 内置 Mods（已把部分功能迁为 Mod）

| 名称 | 作用 |
|------|------|
| cc-plugin-agents-md | 把 AGENTS.md 作为项目指令加载 |
| cc-plugin-diff | 接管 `/diff` 并画面板 |
| cc-plugin-plugin-authoring | 给 Claude 提供写 Mod 的 skill |
| cc-plugin-sec-default | 企业安全兜底，**用户关不掉** |
| cc-plugin-telemetry | 发送分析数据 |
| cc-plugin-you-should-know | 旁路 Agent 盯长任务，漏掉问题时输入框上方提醒（默认关，`/plugin enable cc-plugin-you-should-know@builtin` 开启，限直连 Anthropic 且开遥测） |

- 源码公开在 `anthropics/claude-code` 仓库 `mods/` 目录
- 2.1.287 版本提示词文件少 13 个、token 少 8328 个（-26.6%），推测功能迁入 Mod

### 运行环境支持

| 环境 | Hooks 运行 | Mod 界面显示 |
|------|-----------|-------------|
| 终端 claude（含编辑器内置、JetBrains） | ✅ | ✅ |
| 桌面端 Code 标签页（除 WSL） | ✅ | ✅（仅终端标记元素除外） |
| 桌面端 WSL 会话 | ❌ | ❌ |
| VS Code 扩展聊天面板 | ✅ | ❌ |
| `claude -p` / Agent SDK | ✅ | ❌ |
| Remote Control (claude.ai/手机) | ✅（本机会话） | ✅（本机终端） |
| 云端会话 | 插件能到达时 ✅ | ❌ |

### 权限与安全

- Mod 权限 = 你的权限：读写文件、启动程序、网络请求、读环境变量/API key、看到所有 prompt/工具调用并改写、冒充你提交 prompt、不问批准工具调用、用你套餐/API key 调模型花钱
- **只装信得过的来源**，`claude plugin validate` 先看它会做什么
- 关闭粒度：单个 Mod 禁用/卸载、本会话 `--safe-mode`、全局 `disableAllHooks: true`（组织管理的照常）

### 企业管控

- Team/Enterprise：owner 后台设置 marketplace 允许/屏蔽；第三方 API 套餐用 managed settings 推送
- 有 managed settings 机器上，`cc-plugin-sec-default@builtin` 最先加载，**用户关不掉**
  - 保护：托管 hooks 输入/决定、系统提示词、托管 CLAUDE.md、托管 MCP server 工具、被 deny 规则拒绝的调用
  - 更严可开 `allowManagedModsOnly: true` 只让组织 Mod 加载
- 团队自定义 Mod 例子：CI/CD 状态面板、生产配置前确认、审计日志 Mod

### Mod vs Hooks vs Skills vs MCP

| | Mod | Settings hook | Skill | MCP server |
|--|-----|---------------|-------|------------|
| 是什么 | 插件内 JS/TS 函数 | shell/HTTP/prompt | 指令 .md 文件 | 外部工具服务 |
| 能改什么 | 工具/ prompt/命令/轮次/界面 | 是否放行、工具参数/结果 | Claude 知道/做什么 | Claude 有哪些工具 |
| 能画界面 | ✅ | ❌ | ❌ | ❌ |
| 写法 | JS/TS | 脚本+settings.json | Markdown | 任意语言 |
| 选它当 | 要面板/输入框栏/自定义命令/改写事件 | 用现成脚本拦截/放行/记录 | 老在对话里粘同一段指令 | Claude 需连外部系统 |

- hooks 未废弃，settings 与插件 hooks.json 并存
- 入门建议：从 Token Weather prompt 改起
## 实操补充：一段中文做出 Token Weather（参宿ag）

> 收藏自微信公众号「参宿ag」· 2026-10-07 · https://mp.weixin.qq.com/s/QQXOmq_-RdjdjcIZoAzwrA
> 基于 Addy Osmani《Getting started with Claude Code mods》（2026-10-01，https://claude.dev/blog/getting-started-with-claude-code-mods/）+ 官方文档 https://code.claude.com/docs/en/plugins/mods/overview
> 实测环境：Claude Code v2.1.289 / Sonnet 5.5 / Claude Pro，2026-10-05

### 正式版相对早期测试的变化
- 2.1.287+ 默认开启；早期的 `CLAUDE_CODE_ENABLE_FUNCTION_HOOKS` 可删，新版忽略它，设 0 也关不掉
- 判断是否该用 mod：这件事**需要画在界面上**，或**需要在 Claude 动手之前插一脚**吗？
- `cc-plugin-you-should-know` 补充细节：标签分 *You should know*（需理解的概念）/ *Heads up*（Claude 顺手做、回复没强调的决定，如给 /ask 加缓存反让单次用户多花 25%）；按 2 展开解释；会读整段对话（额外耗额度）并上报选项反馈；旁路 agent 不能读文件/跑命令；`--safe-mode`、`disableAllHooks` 关不掉，只能在 /plugin Installed 里禁用
- Built-in 插件只能启用/禁用，不能卸载、不能更新

### 六步实测流程
1. **贴中文描述**：只写「想看到什么」（5 档天气+颜色、百分比+`134.4k / 200k`、12 轮 `▁▂▃▄▅▆▇█` 折线、`▲ +98.3k 上一轮`、每轮结束更新），不写实现
2. **Claude 自写自验**：自动加载 plugin-authoring skill → 代码写到 `~/.claude/dev-mods/` 临时目录 → `claude plugin validate` 通过、5 个单测通过；主动说明未做的事（没在真会话看效果、没跑 tsc，因类型文件要 mod 加载后才生成）
3. **热重载**：轮末弹 `Enable hot reloading for this session?` → 选 Enable，带子立刻出现。坑：1M 窗口下 85.4k 只占 8%，天气会一直晴；要更敏感就让它「档位按 200k 算」
4. **改代码不用重启**：补跑 tsc 修两处 possibly-undefined，显示 `token-weather: reloaded`，数据不丢；`turn.complete` 读用量、`ui.render` 画带子，所以首轮结束前不显示
5. **补测试**：模拟三轮（36.1k→134.4k→182k）断言 `↯ 即将压缩 91%`；边界：首轮前不显示、压缩后读不到用量不记录、子 agent 轮次不计入；5→9 项全过。人负责肉眼验收颜色/图标
6. **装成正式插件**（最易漏，dev-mods 目录只当前会话有效、之后会被清理）：复制到插件目录 → 加 `marketplace.json` 注册本地 marketplace → `claude plugin install token-weather@token-weather` → 新位置重跑校验（10 项过）→ **删掉临时旧副本**（否则带子重复）
   - 以后改代码**必须调高 plugin.json 的 version**，再 `claude plugin marketplace update token-weather` + `claude plugin update token-weather@token-weather`，否则 update 看不到新版

### 试用官方示例
```bash
git clone https://github.com/anthropics/claude-code-playground
cd claude-code-playground/claude-code/mods
claude --plugin-dir ./blast-radius   # 仅本次会话生效；换 replay-theater / token-weather
```
- Blast Radius 官方演示：拦下 `rm -rf build`，列出 9 个文件共 1.1 MB

### 安全补充
- mod **不在沙箱里**：开了沙箱也只隔离 Claude 执行的 Bash，mod 自己起的进程照样在外面；唯一碰不到的是权限确认弹窗的显示内容
- 装前 `claude plugin validate ./some-mod`（只读不运行）：看 `hooks:`（监听哪些事件）和 `calls:`（调用哪些能力）；只显示天气却要发网络请求 → 要警惕
- 分享：GitHub 仓库 + marketplace 文件，或提交到 Claude 插件目录

### 三个练手点子（只写想看到什么）
- 转圈提示后显示本轮工具调用次数（「Thinking · 工具调用 3 次」，官方入门示例，十几行）
- 输入框上方显示当前 git 分支 + 未提交文件数，每轮更新
- 每轮结束显示本轮耗时，超 3 分钟标黄
