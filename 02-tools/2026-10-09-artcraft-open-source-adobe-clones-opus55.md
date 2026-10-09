# ArtCraft：开发者用 Claude Opus 5.5 连造 7 款开源软件对标 Adobe 全家桶（CSDN）

- **日期**：2026-10-09
- **来源**：微信公众号（CSDN，整理：屠敏）
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/zgbqAEse23l0a14mfGwY7g
- **项目**：https://github.com/storytold/artcraft（8.3k Star / 1.2k Fork；MIT 或 Apache-2.0；官方定位 Alpha）

## 概述

前 Square 工程师 Brandon Thomas 带四人团队，借助 Claude Opus 5.5 用 Rust 开发了 7 款开源应用，对标 Adobe Creative Cloud 的核心产品。他在 Hacker News 上抛出「Software is over」（软件时代结束了）的判断，引发争议：有人称赞 AI 编程的潜力，也有人下载后直言"根本不能用"。

## 7 款应用

| 应用 | 对标 | 用途 |
|------|------|------|
| PhotoCraft | Photoshop | 图片编辑 |
| VectorCraft | Illustrator | 矢量设计 |
| FilmCraft | Premiere | 视频编辑 |
| LightCraft | Lightroom | 照片管理、RAW 开发 |
| EffectCraft | After Effects | 视觉特效与动态图形 |
| DesignCraft | InDesign | 页面排版 |
| PdfCraft | Acrobat Pro | PDF 处理 |

- 界面布局和快捷键贴近 Adobe 使用习惯；支持 CLI、JSON 控制通道和 MCP，便于 AI Agent 执行编辑操作
- macOS / Windows / Linux 原生运行，部分产品可经 WebAssembly 在浏览器运行
- LightCraft：非破坏式 RAW 编辑（曝光/色彩/色调曲线、画笔/渐变/颜色范围蒙版），已解码 DNG/CR2/ARW/NEF/RAF/RW2，部分压缩格式仍有限制
- 项目 ArtCraft 最初（文中称 2015 年成立）定位为「面向艺术家的可控 AI 工具」，近期转向这套开源套件
- 采用 clean-room 方式：通过公开的功能、界面和操作表现了解行为，再独立编写代码，不复制 Adobe 源码

## 开发者的主张

- 起因：受够了 Adobe 的订阅和"取消费用"机制。背景：2024-06 美国政府起诉 Adobe（部分年度订阅提前取消需付剩余月费 50%、取消流程设障）；2026-03 Adobe 同意支付共 1.5 亿美元和解（7500 万民事罚款 + 7500 万免费服务），但否认不当行为
- 自述 15 年经验（高可靠支付系统、机器人与光学自动化）、近十年 Rust
- 功能对等时间表有收缩：先说"一个月内 100% 对等"，几天后改为"99% 对等以月计、不是以年计"
- 激进愿景：AI 让软件护城河消失，可以 clean-room 重建一切，包括 Google 搜索、智能手机；"互联网将回到 1990–2004 年的独立互联网时代，一切以开源形式重获新生"

## 实测与争议

- Hacker News 网友："这是一款人工智能开发的、漏洞百出的软件，标题和描述与事实相去甚远"
- 设计/影视从业者试 PhotoCraft：文字选择位置与鼠标不一致、字号调整无反应、卡顿、文件夹无法展开、快捷键失效；指出 GitHub 当时仅两名提交者，代码量大不等于 Adobe 级成熟度
- Gizmodo 记者："像是在使用一个卡顿但真实的 Photoshop"
- 法律：功能层面的 clean-room 实现可能有合法空间，但 Ars Technica 指出，若整体视觉和界面设计过于接近 Adobe，仍可能有法律风险
- 正面看法：过去一个人不可能同时从零做 Photoshop/Illustrator/Premiere/Lightroom 等一整套，现在至少能快速做出第一版再靠社区迭代

## 备注

- 文章结论：远谈不上取代 Adobe，"能运行"和"能用"差得很远，但小团队借助 AI 同时启动一整套软件已成现实
- 参考：[Ars Technica](https://arstechnica.com/ai/2026/10/software-is-over-bold-ai-developer-takes-aim-at-adobe-with-open-source-clones/)、HN [49958850](https://news.ycombinator.com/item?id=49958850)、[49922569](https://news.ycombinator.com/item?id=49922569)
- 以上均为原文转述，Star 数、团队规模、时间线未独立核实
- 续集：同团队 10-07 再开 5 个仓库重写 Word/Excel/PPT/CAD/Pro Tools，见 [ArtCraft 又用 Rust 重写 Word/Excel/PPT/CAD/Pro Tools](2026-10-09-artcraft-office-cad-daw-rust-rewrite.md)
