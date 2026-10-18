# Agent 的 3D 工作室：15 个 GitHub 仓库串起生成 → 搭景 → 做游戏 → 上网页（码农猿星球）

- **日期**：2026-10-17
- **来源**：微信公众号（码农猿星球）
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/VsE-xzQ-KVkSRSdhVCnnRw
- **相关**：本库 [Hermes + Blender MCP 3D 任务](2026-08-15-hermes-blender-mcp-3d-task.md)、[DSH：无头 Blender AI 3D 建模](2026-10-07-dsh-blender-headless-ai-3d-modeling.md)、[Modly：本地照片转 3D](2026-08-16-modly-photo-to-3d-local.md)

## 概述

一张清单，把微软、腾讯、Stability、NVIDIA、Meshy、PlayCanvas 等机构的开源 3D 能力，按"你想做什么"排成四段：**生成 → 场景 → 游戏 → Web**。原标题"NVIDIA and Microsoft just open-sourced…"更像情绪钩子——真正完整的是一条**可组合的开源 3D 工作流**，而不是某个厂商的一键工作室。

Agent 时代的变化：资产生成越来越快（图/文生 3D）；编辑器开始通过 **MCP** 暴露给 Agent；Web 3D 有成熟运行时。缺的是把这些环节串成可重复的流水线。

## 15 个仓库

### 1. 生成 3D 资产

| # | 项目 | 作用 |
|---|---|---|
| 01 | **TRELLIS.2**（Microsoft） | 单图 → 带纹理 3D 资产；H100 上低分辨率约 3 秒量级 |
| 02 | **Hunyuan3D-2.1**（腾讯） | 面向生产的 3D 资产，强调 PBR 材质 |
| 03 | **stable-fast-3d**（Stability AI） | 单照片 → 干净 UV 展开网格，偏速度 |
| 04 | **meshy-mcp-server**（Meshy） | 文/图生 3D，支持重拓扑、重贴图、绑定、动画，可从 Agent 调用（商业服务） |

怎么选：自建管线、要速度与分辨率档位 → TRELLIS.2；要 PBR、更接近能进制作 → Hunyuan3D-2.1；要 Agent 一站式改网格/材质/绑定 → Meshy MCP。

### 2. 搭建场景与世界

| # | 项目 | 作用 |
|---|---|---|
| 05 | **mcp-for-blender** | 在运行中的 Blender 里用提示词建模、搭场景、改物体 |
| 06 | **HY-World-2.0**（腾讯） | 文本或单图 → 可漫游的 3D 世界 |
| 07 | **supersplat**（PlayCanvas） | 浏览器里检查、编辑、优化、发布 3D Gaussian Splat |
| 08 | **kit-usd-agents**（NVIDIA） | 自然语言编辑 Omniverse USD 场景，含 Kit / Isaac Sim 相关 MCP |

分工：Blender MCP 做精细资产与场景编辑；世界模型出大场景/可导航空间草稿；Splat 工具做高斯表示的预览与发布；USD Agents 面向工业级/仿真场景。

### 3. 做游戏（编辑器进 Agent）

| # | 项目 | 作用 |
|---|---|---|
| 09 | **unity-mcp** | 自然语言驱动 Unity Editor：场景、C#、资产、测试、构建 |
| 10 | **godot-ai** | 约 46 个工具对接运行中的 Godot 编辑器：场景、脚本、信号、UI、粒子 |
| 11 | **unreal-engine-mcp** | 驱动 Unreal Editor：Actor、世界、蓝图等 |

从"AI 写脚本、人手动粘贴"升级到"AI 直接操作编辑器"。

### 4. 3D 上网页

| # | 项目 | 作用 |
|---|---|---|
| 12 | **editor-mcp-server**（PlayCanvas） | Agent 搭建 PlayCanvas 场景，并检查是否真能跑 |
| 13 | **three.js** | Web 3D 基础库 |
| 14 | **react-three-fiber** | 用 React 组件方式写 three.js 场景 |
| 15 | **threejs-skills** | 给 Claude Code 等用的 three.js 技能包（着色器到后期） |

适合产品页、配置器、轻量展示、营销 3D，不适合一上来做重型游戏。

## 三个要点

1. **生成容易，过生产难**：真正耗时的是拓扑、材质一致性、缩放轴向、动画绑定、引擎导入与性能。"检查 → 修复几何/材质/动画 → 再进引擎"才是生产瓶颈。
2. **MCP 改变的是控制权**：以前 Agent 只能输出文件路径和代码，现在能"看编辑器状态 → 修改 → 再验证"。
3. **许可要自己审**：清单混有 MIT、社区许可、研究发布和商业服务（Meshy）；商用前核对模型许可、输出版权、是否允许闭源产品、地区限制。

## 最小可行管线（MVP）

不要一次装 15 个，先跑通：
1. **资产**：TRELLIS.2 或 Hunyuan3D-2.1（二选一）；
2. **清理**：Blender + MCP；
3. **落地**：Unity MCP（游戏）或 Three.js/R3F（网页）二选一。

跑通"图 → 模型 → 进场景 → 能看见/能操作"后，再加世界生成、Splat、第二引擎。

**分工**：Agent 负责调度与迭代，人负责审美、范围和许可。

## 备注

- 原文未给出各仓库链接、星数和许可细节，以各 GitHub 仓库当前页面为准。
