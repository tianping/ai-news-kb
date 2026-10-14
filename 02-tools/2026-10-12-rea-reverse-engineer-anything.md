# REA（Reverse Engineer Anything）：给 AI Agent 接上逆向工具链，闭源客户端被「扒干净」

- **日期**：2026-10-12（收藏）
- **来源**：微信公众号「一人协议」
- **分类**：02-tools（AI 工具 / Agent / MCP）
- **原文**：https://mp.weixin.qq.com/s/TDC4DqlGXSybrSVPtZHvtw
- **项目地址（官方）**：https://github.com/morluto/rea

## 概述

REA 是一个开源 CLI + MCP server，本身不做逆向，而是把 Ghidra、Hopper、IDA、JADX 等专业逆向工具包装成 MCP 工具，让 Claude / Codex / Cursor 等 Agent 直接调用。作者称该项目在 GitHub 一周内从不到 1 万星涨到十几万星，并用剪映的部分本地逻辑、几个闭源小工具和一个老桌面端做了实测。

## 工作流程

1. 输入：没有源码的程序、Electron 桌面端、安卓 APK、老游戏等
2. AI Agent（Claude / Codex / Cursor）用自然语言接收任务，如「这个搜索功能怎么实现的」
3. 通过 MCP 调用逆向工具：拆模块、看路由、查 IPC 通信、读伪代码和汇编
4. 输出：证据（代码线索、调用关系）+ 结论；还可以接着让 Agent「按这个逻辑在我的项目里写一个类似的」

核心价值：把「我猜」变成「我看到了证据」。逆向从需要装工具、读汇编的手艺活，变成会跟 AI 说话就能做。

## 作者实测的四点感受

- **复现功能**：看到别人做得好的交互，让 Agent 顺着模块、路由、通信一路查下去，基本能摸清逻辑，再在自己的项目里照着实现
- **Electron / 桌面端几乎是裸奔**：打包后的前端逻辑、IPC、部分业务代码，对 REA + Agent 来说没什么秘密
- **老软件、老游戏、源码丢失的库**（作者认为最有价值）：Agent 调用 Ghidra 读伪代码，能还原很多核心算法
- **底层还是老工具**：REA 只是「接线员」，把 Ghidra、IDA 的输出结构化，方便 Agent 推理和追问

## 局限 / 踩坑

- 工具链配置麻烦，本地环境没准备好时尤其如此
- 加密很重的软件、或以服务端逻辑为主的软件，只能拆到客户端能看到的部分
- 用官方账号做这类事有风险

## 作者的判断：闭源客户端的护城河还剩多少

- 「把逻辑藏在二进制里」已经不再是壁垒，闭源客户端的很多逻辑不再天然不可见
- 真正值钱的东西要往服务端、数据、网络效应、品牌、交付速度上挪
- 学习成本被压低：普通人也能快速理解别人怎么实现某个功能，试错速度更快
- 工具再强，判断哪些能用、哪些不能碰，还是人来拍板

## 备注（2026-10-12 补充）

- **官方仓库是 `morluto/rea`**（README：可用已安装的 Hopper / Ghidra / IDA 做原生分析，静态 JS 分析不需要原生引擎；setup 会把 REA 的 MCP server 和配套工作流说明写入所选 Agent 的配置，并先备份原有配置）。
- ⚠️ 搜索结果里另有一个 `github.com/ReverseEngineerAnything/REA`，标题堆满「github rea / official free download / Windows zip」这类关键词，是典型的仿冒 SEO 仓库，很可能借热点分发恶意安装包，**不要从那里下载**。
- 原文图片是「界面重构示意」，不是剪映等产品的真实分析截图；作者也声明文章不代表对任何软件的完整审计，逆向分析须遵守法律、许可和授权边界。

## 接入教程（2026-10-14 补充）

- **来源**：微信公众号「千秋英雄录」《保姆级教程：把 REA 接入 Claude Code、Cursor、Devin 等 10+ Agent，10 分钟搞定》（https://mp.weixin.qq.com/s/9_PLUnoVmXuAyhGgtqxivA）

**环境**：Node.js 22.x（≥22.19）/ 24.x（≥24.11）/ 26+；macOS 或 Linux（Windows 部分功能仍在完善）。原生二进制深度分析需另装 Hopper（macOS）或 Ghidra（Linux/macOS）；只分析 JS / Electron / .NET / 网站则不需要。

**方式一：自动配置（推荐）**

```bash
npx rea-agents setup                # 或 npx --yes rea-agents@latest setup 跳过交互
```

1. setup 扫描本机已装的 Agent，多选要接入的（已配置过的默认勾选，检测到但未配置的需手动勾）
2. 先展示拟议变更（改哪个文件、写什么），确认后才写入，并**自动备份原配置**
3. **重启**所选 Agent（MCP 注册必须重启才生效）
4. 验证：`npx -y @morluto/rea doctor`（检查 Node、npm、Agent 集成）

| Agent | 自动配置 | 写入位置 / 备注 |
|---|---|---|
| Claude Code | ✅ | 项目根 `.mcp.json`，hooks 与 slash commands 复制到 `.claude/`，注入引导式工作流指令；重启后 `/mcp` 看到 rea 已连接 |
| Claude Desktop | ✅ | `claude_desktop_config.json`（通常 `~/Library/Application Support/Claude/`），需完全退出再重启 |
| Codex | ✅ | 额外 30 秒冷启动容忍（包运行器加载偏慢） |
| Cursor | ✅ | `~/.cursor/mcp.json` 或项目 `.cursor/mcp.json` |
| Gemini CLI | ✅ | 对应 MCP 配置，重启终端会话 |
| Windsurf | ✅ | `mcp_config.json`（Settings → Cascade → MCP Servers） |
| Devin | ⚠️ 仅检测 | 需手动：用户级 `~/.config/devin/mcp_config.json`（Win：`%APPDATA%\devin\`）、项目级 `.devin/mcp_config.json`、本地级 `.devin/mcp_config.local.json`；项目级覆盖用户级 |

**方式二：手动配置**（任何支持本地 MCP server 的 Agent）

```json
{
  "mcpServers": {
    "rea": { "command": "npx", "args": ["-y", "rea-agents@3.1.0", "mcp"] }
  }
}
```

⚠️ 持久注册**写精确版本号**，不要写 latest——REA 迭代快、版本间可能有破坏性变更；`rea setup` 会自动锁版本并在升级时同步。

**确认接好**：`rea doctor` → `npx -y rea-agents@latest capabilities`（CLI 能列出能力即正常）→ 在 Agent 里问"用 REA 列出当前可用的逆向分析能力"。

**可选全局安装**：`npm install --global rea-agents && rea setup && rea doctor`，之后不用 npx 前缀。

**避坑**：手动配置锁版本号；手改前自己再备份；Devin 必须手动；Agent 看不到 REA 工具时先查是否忘了重启。

## 相关笔记

- [REA + Reverse-skill + PentAGI：让 Codex 1:1 复刻任何软件功能（草稿鸭）](2026-10-13-rea-reverse-skill-pentagi-clone-pipeline.md) — 逆向→复刻→安全体检的完整流水线
- [用 REA 逆向复刻 Microsoft Word（杉森楠）](2026-10-13-rea-reverse-engineer-microsoft-word.md) — 实测：Pro 会员约 20%～30% 额度复刻出界面近像素级的 Word，提出「AI 复刻钱效比」
