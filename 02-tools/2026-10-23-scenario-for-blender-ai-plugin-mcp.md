# Scenario for Blender：一个插件打通文生图/视频/3D/PBR/音频/HDRI，附本地 MCP（三次方AIRX）

- **日期**：2026-10-23
- **来源**：三次方AIRX
- **分类**：02-tools 工具与产品（3D / Blender）
- **URL**：https://mp.weixin.qq.com/s/imd13812b5sMmSYhi5UcmQ
- **教程**：https://blender.scenario.com/
- **GitHub**：https://github.com/scenario-labs/blender-plugin

## 概述

AI 生成平台 Scenario 为 Blender 推出官方插件 **Scenario for Blender**，把 AI 图像、视频、3D 模型、PBR 材质、音频、360° 环境光六类生成能力放进 Blender 3D 视口的一个侧边栏面板。插件还自带本地 MCP 服务器，让 Claude Code、Cursor 等 Agent 直接读写场景。和独立 AI 网站相比，最大的区别是**它认识你的场景**：输入来自工程本身，生成结果自动装回工程。

## 六个生成通道

| 通道 | 能力 | 结果落点 |
|---|---|---|
| 文生图 / 图生图 | 一句话出图 | 可作为贴图接到材质节点 |
| 文生视频 | 可用当前视口截图做参考 | 氛围成片 |
| 文生 3D | 描述生成可用模型；还能把文字描述直接搭成灰盒（Greybox）场景骨架 | 放在 3D 光标处或原物体旁边 |
| 文生 PBR 材质 | 一键生成整套 PBR 贴图 | 自动贴回所选模型的对应材质槽 |
| AI 音频 | 音效、背景音乐 | 铺进时间轴音序器（Sequencer） |
| 360° 环境光 | HDRI | 写进场景的环境设置 |

网友的算法：（Midjourney + Runway + Tripo/Meshy + 材质库 + 音效库 + HDRI 库）÷ 一堆网页 = 一个 Blender 侧边栏。

## 场景感知

- 输入可以是视口截图、场景相机、动画预演片段、当前选中的网格
- 用相机路径做输入，生成画面跟着运镜走；选中网格生成材质，直接贴回那个模型
- 2D 出图是终点，3D 里出图只是中间件。只有生成结果作为数据块、纹理、网格或音序器片段自动装回工程，AI 才算真正进入生产管线
- 每次生成前先显示要消耗多少 Creative Units，先报价再下单

## 现有资产改造

内置网格编辑工具：重拓扑、重新贴图、UV 展开、骨骼绑定、动画、拆分成多个部件。

## 上手条件

- Blender 5.0+（官方目标支持 5.0 / 5.1 / 5.2）
- Scenario 账号并开通 API 访问（需要 API Key 和 Secret）；按 Creative Units 计费
- 联网：在 Edit > Preferences > System > Network 里打开 **Allow Online Access**
- 安装：官方扩展仓库在线装（推荐，可自动更新），或下载 ZIP 手动装
  - ⚠️ ZIP 版和仓库版是两个独立安装体，凭据和任务记录互不迁移，不要同时装两份

## 备注

- 本质上不是新模型，而是把断开的流水线焊进视口，生成能力来自 Scenario 云端的付费服务
- 可以和本库的 [Agent 的 3D 工作室：15 个 GitHub 仓库](2026-10-17-agent-3d-studio-15-github-repos.md) 对照：那篇里的 Blender MCP 是开源方案，Scenario 是商业一体化方案
