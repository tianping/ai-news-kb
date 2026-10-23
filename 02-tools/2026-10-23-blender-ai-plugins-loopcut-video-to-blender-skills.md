# Blender 四个 AI 插件：LoopCut、Scenario、video-to-blender、Blender Skills（AI for Frontend）

- **日期**：2026-10-23
- **来源**：AI for Frontend
- **分类**：02-tools 工具与产品（3D / Blender）
- **URL**：https://mp.weixin.qq.com/s/iy1t9nmAA4aCWQRW3qs9OQ

## 概述

很多人觉得用 Agent 连 Blender 官方 MCP 建模太费 tokens，于是出现了一批在 Blender 内直接调 AI、或给 Agent 提供专业流程封装的工具。本文介绍四个：把 AI 代理做进 Blender 分支的 **LoopCut**、商业生成平台插件 **Scenario for Blender**、把参考视频转成程序化节点图的 **video-to-blender**、22 个专业 3D 流程技能包 **Blender Skills**。

## 1. LoopCut：内置 AI 代理的 Blender 分支

- 官网：https://loopcut.org
- 基于 Blender 5.2 的完整分支，多了一个 AI 面板，**独立安装包**，不是插件
- 工作方式：用自然语言描述任务 → 读取当前场景 → 生成真实的 bpy 代码 → **先展示代码，你批准后才执行** → 用视口截图检查结果；有完整检查点，可以回退（比普通 Undo 更强）
- 常见任务：
  - 命名整理：按材质重命名、把灯光放进独立集合、清理孤儿数据
  - 材质：批量设置和调整
  - 灯光相机：搭三点布光、匹配参考图构图
  - 渲染：Cycles / EEVEE 参数、输出预设、批量渲染
  - 建模辅助：应用缩放、修复翻转法线、整理修改器顺序、加 IK、镜像命名
  - 资源库：搜索并导入 Poly Haven 等免费模型、材质、HDRI
  - 云端服务（需 LoopCut 账号）：图片转 3D、云端渲染、语音 / 音效生成
- 模型选择：
  - 用自己的 ChatGPT Plus/Pro 登录（推荐，不额外收费）
  - 自带 API Key（Anthropic / OpenAI 兼容）
  - 本地模型（Ollama、LM Studio）
  - LoopCut 自带模型（有免费额度）
- 适合整理场景、布光、渲染设置、批量操作这类重复的技术活，让人专注于建模和创意

## 2. Scenario for Blender（补充上一篇未提到的细节）

- 详见本库 [Scenario for Blender](2026-10-23-scenario-for-blender-ai-plugin-mcp.md)
- 本文补充：
  - **Render Image / Video**：用当前场景截图加风格参考，生成静态图或视频
  - **Film tasks**：用 JSON 配方生成镜头、时间线、审片场景，并导出视频
  - **本地 MCP 默认地址**：`http://127.0.0.1:9876/mcp`，可供 Claude Code、Cursor、Codex 调用

## 3. video-to-blender：参考视频 → 可编辑的程序化节点

- GitHub：https://github.com/gokayfem/video-to-blender
- 让 AI 编程助手（Claude Code、Cursor、Codex、Gemini）根据参考视频、图片或文字，生成真正的 Blender **原生节点图**，而不是烘焙死的网格或关键帧：
  - Geometry Nodes：几何、布局、运动（街道、房子、人物走路、旗帜飘动）
  - Shader Nodes：材质
  - World Shader + Compositor：天空、大气、后期
- 交付物是一个 `.blend` 文件，里面是干净、有组织的节点组
- 示例「Storybook Street」：参考是约 20 秒的 AI 动画（兜帽角色走在半木结构的石板街上）。整条街被重建成一个 Geometry Nodes 修改器，换一条路径曲线就能生成不同的街道布局；房子宽度、楼层、道具密度、旗帜摆动、人物步态都可调。文件约 1MB，参考视频 19MB 以上
- 作者前作是 Video 2 GLSL（把视频变成纯数学的 GLSL shader），这次升级为艺术家可以打开、修改、复用的程序化工具
- 一句话：把「看起来像什么」变成「可调参数的节点图」

## 4. Blender Skills：22 个专业 3D 流程 Agent 技能

- GitHub：https://github.com/EL4CTEO/blender-skills
- 面向 Claude Code、Codex、Gemini CLI、GitHub Copilot、Cursor，针对 **Blender 5.2 LTS+**
- 解决的问题：普通 AI 写 bpy 常出错（API 版本、最佳实践、游戏资产规范不对）。每个技能有 SKILL.md 说明和可执行脚本，Agent 加载后按正确步骤操作
- 两种执行方式：通过 Blender MCP Server 控制当前打开的 Blender，或在没有 MCP 时后台 headless 运行。所有脚本都在真实 Blender 里测过（50 个 headless 测试）

| 类别 | 技能 |
|---|---|
| 基础 | blender-core（入口、路由、5.x API 规则）、场景设置 |
| 建模 | 硬表面、雕刻、Geometry Nodes、重拓扑 + UV |
| 材质贴图 | 正确的 PBR 材质；法线 / AO / 曲率 / ORM 烘焙 |
| 角色 | Rigify 绑定、ARKit 52 面部表情、动画（含根运动） |
| 游戏优化 | LOD、预算、导出（Unity / Unreal / Godot / Roblox / Web / USD） |
| 渲染特效 | Cycles / EEVEE、灯光 lookdev、刚体 / 布料 / 流体模拟 |
| v1.1.0 新增 | 时间与天气：黄金时刻、夜晚、雾、雨雪、湿地面、水坑、闪电 |
| 质量 | 资产 QA：检测 z-fighting、浮动零件、法线和权重问题 |

- 示例提示词：
  - 「做一个游戏可用的木箱，烘焙法线，加 LOD，导出到 Unreal」
  - 「给这个角色用 Rigify 绑定，再导出到 Unity，带走路循环」
  - 「检查这个模型有没有 z-fighting 和浮动零件，然后导出到 Roblox」
  - 「把这条街做成雨夜，有水坑和闪电」

## 选型速记

| 需求 | 选择 |
|---|---|
| 在 Blender 界面里用自然语言做技术杂活，代码先审后执行 | LoopCut |
| 一站式 AI 生成图 / 视频 / 3D / 材质 / 音频（付费） | Scenario for Blender |
| 把参考视频或概念变成可调参的程序化场景 | video-to-blender |
| 让 Claude Code 等 Agent 按专业规范完成建模→绑定→导出 | Blender Skills |

## 备注

- 本库其他 Blender 笔记：
  - [程序员视角的 Blender 入门](2026-10-23-blender-for-programmers-agent-automation.md)
  - [Blender 无头 AI 建模](2026-10-07-dsh-blender-headless-ai-3d-modeling.md)
  - [Hermes + Blender MCP](2026-08-15-hermes-blender-mcp-3d-task.md)
- 项目介绍来自原文转述，未逐一验证；LoopCut 是第三方 Blender 分支，安装前确认来源，不要覆盖已有 Blender
