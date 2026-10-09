# ArtCraft 又用 Rust 重写 Word/Excel/PPT/CAD/Pro Tools（浩哥AI笔记）

- **日期**：2026-10-09
- **来源**：微信公众号「浩哥AI笔记」
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/WoTIpTVPDwzZUh03ka9k8Q
- **前情**：本库已收 [ArtCraft：用 Opus 5.5 连造 7 款开源软件对标 Adobe](2026-10-09-artcraft-open-source-adobe-clones-opus55.md)，本篇是续集——同一团队 10-07 一口气再开 5 个仓库，对标 Office / AutoCAD / Pro Tools

## 概述

ArtCraft 团队（GitHub 组织 storytold）此前用纯 Rust 重写 Adobe 系列，作者称 Photoshop 那个已拿到 17000+ Star（前一篇文章写的是 8.3k，时间点不同）。10 月 7 日建仓、次日发 v0.3.0，新开五个仓库。作者的结论：看热闹可以，拿来干活还早；这五个连 alpha 都没到。

## 五个新项目（星数/Fork 为作者写稿时数据）

| 项目 | 对标 | Star / Fork | 要点 |
|------|------|-------------|------|
| WordCraft | Word | 850 / 307（42 个 open issue） | 功能区、样式、表格、修订、参考文献、邮件合并；直接读写 .docx；macOS/Windows/Linux/BSD + WebAssembly 浏览器运行；自带示例文档 The Open Studio Handbook |
| GridCraft | Excel | 578 / 257 | 500+ 函数、动态数组/LET/LAMBDA/结构化引用；17 种图表；PivotTable（字段列表、日期分组）；XLSX 原生读写（样式/主题/公式/条件格式/批注/超链接/图表）+ CSV/TSV；Flash Fill；依赖图计算引擎 + 增量重算 + undo 快照 |
| DeckCraft | PowerPoint | 487 / 262 | 150+ 预设形状、连接符、合并形状、渐变/图片/图案填充、Morph 转场；读写 .pptx，导出 PDF/PNG；200+ 命令可脚本化；README 自评：功能广度覆盖 PowerPoint 的 79%、核心功能 93%，**离第一个 alpha 还差约 80% 的路** |
| CADCraft | AutoCAD | 795 / 360 | 图纸空间、视口、标注、剖面视图、标题栏、PAPER 开关；零件图含螺栓孔/中心线/尺寸/注释 |
| SoundCraft | Avid Pro Tools | 557 / 292 | README 标 pre-alpha；完整 DAW：Edit 窗口（波形）、Mix 窗口、每轨 10 插入 + 10 发送、MIDI 编辑器含钢琴卷帘、自带合成器；录音/bounce/插件/发送/自动化全部挂到命令行 |

## 共同套路：给 Agent 留接口

- 官网 getartcraft.com，口号 **No subscription / 无授权服务器 / 无遥测**
- 全部 Rust，Apache-2.0
- 每个应用都带 JSON 控制通道 + MCP server + CLI：人用鼠标点，Agent 也能直接驱动
- 与其"重写订阅制软件"的初衷一致：不被大厂锁定，自己造一套能随便拆的

## 作者的判断

- 涨得快一半是情绪：Office 订阅一年几百美元，CAD 与 Pro Tools 更贵（Pro Tools 年费可上千美元），有人用 Rust 全重写一遍，点进去就是一种表态
- 但全是 0.3.0、出生两天；Office 真正的护城河是插件生态、宏、兼容细节、团队协作，这些都还没有；PSD/DOCX 格式兼容能啃，但对齐到 100% 是一两年的事
- 作者没有本地装过（0.3 太新、机器无 Rust 环境），等 1.0 再写实测；数据核验到 10-09，星数会继续涨

## 备注

- 开源地址：https://github.com/storytold/gridcraft 、https://github.com/storytold/deckcraft 、https://github.com/storytold/cadcraft 、https://github.com/storytold/soundcraft（WordCraft 仓库地址原文未给）
- 文中的 Star/Fork、功能清单、"clean-room"说法均为原文转述，未独立核实；与前一篇同一事件的争议（实测卡顿多 bug、界面近似 Adobe 的法律风险）可对照阅读
