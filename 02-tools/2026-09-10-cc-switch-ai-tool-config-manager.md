# CC Switch：一个应用管 8 款 AI 编程工具的配置文件，再也回不去手改配置了

> 收藏时间：2026-09-10
> 来源：微信公众号「开源软件社」（2026-09-10）
> 原文链接：https://mp.weixin.qq.com/s/5bBnXNwLng2l3b3V3Fl-VA
> 项目：https://github.com/farion1231/cc-switch （farion1231/cc-switch，Rust + Tauri2，MIT，约 12.9 万 Star）

## 解决什么问题

Claude Code、Claude Desktop、Codex、Gemini CLI、Grok Build、OpenCode、OpenClaw、Hermes 各有一套配置格式（JSON/TOML/.env），换 API 供应商就得挨个手改；MCP 服务器和 Skills 散落各处无统一入口；切换供应商后插件配置神秘消失。CC Switch 用「一个桌面应用管八款工具」取代「打开编辑器手改配置文件」。

## 核心能力

- **50+ 供应商预设**：覆盖 AWS Bedrock、NVIDIA NIM 及社区中转，复制 Key 一键导入；「通用供应商」一份配置同步到 Claude Code / Codex / Gemini CLI
- **切换方式**：主界面选中点 Enable，或系统托盘直接点供应商名即刻生效。注意：除 Claude Code 支持热切换外，大多数工具切换后需重启终端/CLI 才生效
- **首启兜底**：可把现有 CLI 配置导入为默认供应商，原 Key/端点/插件不丢
- **共享配置片段（Shared Config Snippet）**：从当前供应商提取 API Key 与端点之外的公共数据，新建供应商时默认勾选写入，解决切供应商后插件配置丢失
- **数据安全**：SQLite 单一数据源 `~/.cc-switch/cc-switch.db`（供应商/MCP/Prompts/Skills）+ `settings.json` 设备偏好；临时文件+重命名原子写入，互斥锁防并发写坏；自动备份保留最近 10 份（Skills 备份 20 份）
- **统一面板**：MCP（双向同步+DeepLink 导入）、Prompts（Markdown 编辑器跨应用同步到 CLAUDE.md/AGENTS.md/GEMINI.md，带回填保护）、Skills（GitHub 仓库/ZIP 一键安装，符号链接或文件复制）
- **代理与故障转移**：本地代理支持热切换、格式转换、自动故障转移、熔断、健康监控，可按应用/供应商粒度接管
- **用量仪表盘**：花费/请求数/Token 趋势+请求日志，自定义模型价格；会话管理器可浏览/搜索/恢复对话历史
- **云同步**：Dropbox/OneDrive/iCloud/NAS/WebDAV；DeepLink `ccswitch://` 导入配置
- **OpenClaw 工作空间编辑器**：直接编辑 AGENTS.md、SOUL.md 等，带 Markdown 预览
- 跨平台原生应用（Win/macOS/Linux），中繁英日多语言；最小侵入设计——卸载 CC Switch 后原生 CLI 仍正常工作

## 安装

- macOS 12+：`brew install --cask cc-switch` 或下载 .dmg（Apple 签名公证）
- Windows 10+：.msi 或便携版 zip
- Arch：`paru -S cc-switch-bin`；Ubuntu 22.04+/Debian 11+/Fedora 34+：.deb/.rpm/AppImage（Flatpak 不在官方发布范围）
- 首启：Add Provider 选预设或自定义 → Enable → 重启终端生效（Claude Code 免重启）

## 适用人群 / 不适用

适合：同时用 2+ 款 AI 编程工具的人；频繁切换 API 供应商（多中转/官方渠道）；AWS Bedrock/NVIDIA NIM 企业团队；把 MCP/Prompts/Skills 当资产经营；多设备办公；在意配置安全（原子写入+自动备份）。
不适合：只用一款工具一个固定渠道——配置一次基本不用再动，收益有限。注意 README 提到的中转服务是第三方商业服务，与开源项目本身是两回事，自行判断。

## 标签

- AI 编程工具、配置管理、Claude Code、Codex、Gemini CLI、开源、Rust、Tauri、MCP、Skills
