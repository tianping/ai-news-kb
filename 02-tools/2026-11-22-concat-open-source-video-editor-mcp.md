# Concat：免费开源本地视频剪辑器，内置 MCP 让 AI Agent 一句话剪片（AI简研社）

- **收藏日期**：2026-11-22
- **来源**：微信公众号「AI简研社」，作者 阿布不讲道理（2026-11-17 发布）
- **原标题**：剪映的天塌了！开源剪辑神器concat一句话剪出爆炸效果
- **分类**：02-tools / 视频剪辑
- **URL**：https://mp.weixin.qq.com/s/UxgV_-pmM5ZeWUsCykfuHQ
- **项目**：[jub0t/Concat](https://github.com/jub0t/Concat) · 官网 https://concat.standalones.org

## 一句话

Concat 是一款对标剪映的**免费开源、纯本地、跨平台**视频剪辑器：不要账号、不加水印、不收会员费，AI 功能（字幕、配音、抠像）也在本机跑；**内置 MCP 服务器和 JSON-RPC API**，Claude 等 AI Agent 能直接驱动它剪片。

## 原文要点

- 痛点：剪映的自动字幕、高清滤镜、去水印、转场要开会员，一年几百块
- Concat 免费开源，原文说 GitHub「18.2 星标」（应指 1.82 万）
- 不登录、不开会员、不上云，飞机上没网也能剪，素材不外传
- 功能：自动字幕、AI 配音、声音克隆、100 种特效、转场、4K 导出
- 杀手锏：「给它一句话，它能自己剪片」，适合批量处理、重复剪辑
- 支持苹果、Windows、安卓
- 不想折腾 GitHub 的，原文推荐让电脑版「百度搭子」代装，提示词：
  > 你好搭子，帮我把GitHub上的jub0t/Concat  SMCFUWD安装到D盘，并在桌面生成快捷方式

  （`SMCFUWD` 原文没解释，可能是推广码，不是仓库的一部分）

## 对照仓库核实（2026-11-22）

| 项目 | 仓库实际情况 |
|------|------|
| Star | 约 19.6K，Rust 编写，仍在活跃更新 |
| 许可 | **AGPL-3.0**（改了再分发要开源） |
| 版本 | 0.2.12，**Beta**，作者自己也说还有毛边 |
| 平台 | macOS（Intel/Apple 芯片）、Windows x86_64（ARM 暂停）、Linux（deb/rpm/AppImage/Arch/Flatpak）、Android、iOS（只能侧载） |
| AI 功能 | 自动字幕、人声分离、TTS + 声音克隆（**仅桌面端**）、背景移除/绿幕/蒙版、画质增强（最高 2K）、防抖；模型在设置里下一次，之后离线可用 |
| 剪辑 | 100+ 特效/滤镜/转场、25 个音效、关键帧 + 曲线编辑、变速曲线、多轨多时间线、标题（含中文预设）、配音降噪 |
| 导出 | H.264 / HEVC / AV1，硬件编码，最高 4K60、10-bit |
| 自动化 | `concat --mcp` 启动内置 MCP（见 docs/MCP.md），另有 JSON-RPC API；源码构建还带 gRPC 和命令行工具 |
| 未签名 | Windows 遇到 SmartScreen 点「更多信息 › 仍要运行」；macOS 用 `xattr -dr com.apple.quarantine /Applications/Concat.app` |

## 点评

- 「一句话剪片」靠的不是软件里自带的智能体，而是**外部 AI Agent 通过 MCP 来操作**。原文说「跟豆包说需求」不太准确，仓库写的是 Claude 等支持 MCP 的 Agent
- 原文是推广向，没有实测截图，也没讲 MCP 怎么配
- 跟知识库里的视频工具比：OpenMontage 和「太上老君炼丹炉」负责从选题写稿到 Remotion 出片，Concat 能补上「可视化精修 + Agent 可调用的时间线剪辑」这一环
- 下载优先走官网或 GitHub Releases，小心仿冒仓库
