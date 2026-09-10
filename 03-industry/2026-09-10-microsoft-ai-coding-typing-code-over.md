# 「敲代码的时代，彻底结束了！」微软 18 年老兵放话，引网友吐槽：怪不得 Win11 这么烂

**来源：** 微信公众号 CSDN（CSDNnews），整理 郑丽媛，2026-09-10
**链接：** https://mp.weixin.qq.com/s/uG8jIyZvATb13XRfKZAD8Q
**参考链接：** https://www.windowslatest.com/2026/09/05/microsoft-distinguished-engineer-says-typing-code-is-absolutely-over-and-windows-11-is-already-being-built-that-way/
**标签：** #AI-Coding #Microsoft #Windows11 #Copilot #Project-Zenith #行业动态

---

## 一句话结论

微软 18 年老兵 David Fowler（SignalR 联合创建者、NuGet/Kudu 创始开发者之一、ASP.NET Core 核心贡献者）在 X 上放话 "Typing code is absolutely over"——他的意思不是开发者会消失，而是 AI 包揽重复性编码后，开发者应把精力转向架构、需求分析、安全审核。网友坐不住的是另一层联想：Win11 那些 Bug，是不是因为微软在用 AI 写代码？

## 从「帮我写代码」到「帮我把软件做出来」

- **微软 CEO 纳德拉**：公司内部已有 20%-30% 的代码由 AI 编写
- 不同于过去以代码补全为主的 AI Coding，微软正把 AI 推向完全不同的方向——参与完整开发闭环的 Agent

**GitHub Copilot Coding Agent 的闭环能力：**
- 接收一个开发任务 → 自行准备环境 → 修改代码仓库 → 运行测试 → 检查构建结果 → 创建 PR 交人类审核
- 今年 2 月为 Coding Agent 增加 Windows 开发环境支持（可直接构建/测试 Windows 项目、运行 Linter、验证 Build）
- Copilot Agent 进入 WSL，参与 Windows + Linux 混合开发

**微软自己的 Aspire（正是 David Fowler 负责的项目）：**
- 通过统一的应用模型、CLI 和 MCP 支持，AI Agent 能获得比单个代码文件更多的上下文
- 甚至能启动服务、查看日志、检查 Telemetry、重启故障组件，然后再次测试

→ 结论：微软正在把 AI 从「代码生成器」变成能参与完整开发闭环的 Agent。

## 让 Windows 本身更适合 AI Agent 开发软件

最近对 Windows 开发工具的调整指向一件很明确的事：**让 WinUI 3 成为新一代原生 Windows 应用的推荐框架，且 WinUI 3 已开源。**

对 AI Coding 来说很关键：AI Agent 要真正参与软件开发，需要大量上下文。
- 封闭、混乱、文档不完整的开发环境，AI 很难理解
- 开源、清晰 API、完善文档、标准化项目结构的框架，更容易被 AI「读懂」

## 连运行 AI 的 Windows 开发机器都准备好了：Project Zenith

9 月 4 日，微软正式推出 **Project Zenith**——为开发者打造的精简版、开发者优化的 Windows 11 运行环境：
- 减少与开发无关的系统干扰
- 预配置 VS Code、GitHub Copilot、WSL、PowerShell 等开发工具，拿到机器就能快速进入 Coding 状态
- **硬件门槛硬核**：至少 64GB 统一内存、250GB/s+ 内存带宽，首批适配 AMD Ryzen AI Halo 芯片

**为什么内存要求这么高？** 为了让开发者在本地直接跑起 300 亿参数以上的大模型，不用再为每次 AI 辅助编程付云端 Token 费。

→ Project Zenith = Windows 11 + 本地大模型 + Copilot Agent + WSL + 开发工具，一个 AI 原生的开发环境。

## AI 写得越多，Win11 真的会更好吗？

- **Windows 内核**目前仍由资深工程师用 C/C++ 严格维护，不太可能贸然交给 AI
- **内核之外**大量 UI 组件、系统服务和配套功能，采用 AI 辅助开发已没悬念

**Win11 近期表现确实难让人放心：**
- 今年 1 月一次大规模安全更新导致部分设备无法正常关机；另一批用户 Remote Desktop 登录彻底失效，微软紧急发布两个带外更新
- 文件资源管理器依旧卡顿，暗色模式 Bug 越修越糟

**用户/开发者反应（情绪表达为主）：**
- "难怪 Win11 每次更新都搞坏这么多东西，怪不得这么烂——原来是 AI 写的代码"
- "几十年的 Win 铁杆，现在被 AI 塞得菜单乱七八糟、后台功能只拖慢系统，考虑转投 Linux"
- "我还是自己敲，至少对每行了如指掌。等公司没人懂代码库、没人会调试 AI 生成的代码，你们只能自求多福"

## 核心观点

- 这些评论多为情绪表达，**目前不能证明 Win11 的问题是 AI 编码导致的**
- 它们反映的是 AI Coding 最大的争议之一：**AI 写得快 ≠ 写得好**
- AI 让代码生成变廉价，**审核和验证反而更重要**——这正是 David Fowler「敲代码时代结束」真正想表达的
- 对 Win11 用户来说更关心的是另一个问题：Bug 横行的时代究竟何时结束

## 关联

- DHH 访谈：Linux 终将统治桌面、手写代码时代终结（"我已经变得可有可无"）→ 03-industry 2026-09-08
- 美国拟封堵中国 AI 海外算力通道、东南亚数据中心或受审查 → 03-industry 2026-09-08
