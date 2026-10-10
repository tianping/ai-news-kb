# ArtCraft 又用 Rust 重写 Word/Excel/PPT/CAD/Pro Tools（浩哥AI笔记）

- **日期**：2026-10-09
- **来源**：微信公众号「浩哥AI笔记」
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/WoTIpTVPDwzZUh03ka9k8Q
- **补充来源（2026-10-10）**：极客BIM设计工坊《CADCraft：用 Rust 重做 AutoCAD 工作流的开源 CAD 工具》（https://mp.weixin.qq.com/s/PazweoIDyWATRMBV0raGFg），见下方「CADCraft 专题」节
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

## CADCraft 专题（极客BIM设计工坊，2026-10-10 补）

起因是 X 上 @Sn0wbrave 的一条阿拉伯文帖子，把 CADCraft 称作 AutoCAD 的开源替代，配图是一张公寓平面图（墙体填充、房间标签、家具线稿、面积表、建筑尺寸、图层与属性面板、命令行）。作者依据官方 README 和 Release 逐项拆解：纯 Rust 实现，有桌面版、浏览器版、CLI 和 MCP 四个入口。

### 复刻的是 AutoCAD 的操作习惯
- 入口仍是命令行：输入 `L` 画线，`@5<45` 表示相对极坐标，配合对象捕捉、正交、极轴追踪控制精度
- 支持命令提示、关键字补全、窗口选择、夹点编辑、右键重复命令
- 图层、标注、填充、块、布局仍围绕二维制图展开，老 CAD 用户不用重新适应

### 文件兼容
- **DXF** R12–2018 读写
- **DWG** R13–2018 读写，通过开源库 acadrust 实现
- PDF 出图、SVG/PNG 导出、纸空间布局、视口、页面设置、PLOT/EXPORTPDF
- 作者提醒：README 只列了支持范围，没有完整的兼容性实测矩阵

### 自动化入口（真正的新东西）
- **cadcraft-cli**：读取图纸信息（info）、转换文件（convert）、运行脚本（run）、导出图像。README 示例：`cadcraft-cli run --sample --script 'CIRCLE 22,3 1' --save out.dxf --export out.png`，即跑示例图纸脚本、存 DXF、导出 PNG（作者注明未独立测试速度和质量）
- **MCP 服务**：命令行输入、JSON 执行、图纸检查、实体查询、渲染
- 意义：从「打开软件手工点」扩展到「通过命令和协议调用」，Agent 能先读当前图纸再操作，渲染结果回到检查流程

### 适合先做什么
- 熟悉 CAD 命令的个人：本地打开 DXF、画二维图、导出
- 批量处理图纸的开发者：先用 CLI 的 info/convert/run 验证读取、脚本、导出链路
- 想让 Agent 操作 CAD 的团队：研究 MCP、JSON 控制通道和图纸检查接口；文件兼容、几何正确性和出图仍需人工复核

### 项目状态
- GitHub 最新 Release **v0.4.0**（10-09 浩哥一文时为 v0.3.0），README 仍标 **early development**
- 正式项目先用自己的图纸测兼容性、标注、字体、块和打印结果，不能等同成熟商业 CAD

## 备注

- 开源地址：https://github.com/storytold/gridcraft 、https://github.com/storytold/deckcraft 、https://github.com/storytold/cadcraft 、https://github.com/storytold/soundcraft（WordCraft 仓库地址原文未给）
- 文中的 Star/Fork、功能清单、"clean-room"说法均为原文转述，未独立核实；与前一篇同一事件的争议（实测卡顿多 bug、界面近似 Adobe 的法律风险）可对照阅读
