# blender-skills：94 个 Blender 技能，把 Claude Code / Cursor 变成技术美术团队（晚风逐云开）

- **日期**：2026-11-18
- **来源**：微信公众号「晚风逐云开」
- **分类**：02-tools 工具与产品（AI + 3D 建模 / 独立游戏美术）
- **原文**：https://mp.weixin.qq.com/s/SXjPaf1LlqL49gDxJrJuAQ
- **仓库**：https://github.com/arjun988/blender-skills（MIT）
- **官网**：https://blender-skills.vercel.app

## 概述

作者的判断：AI 编程工具普及后，独立游戏卡住的地方从代码变成了美术，比如主角模型、场景套件、绑定动画。外包按件收费，自学 Blender 又要半年。blender-skills 不训练生成模型，而是把技术美术的流程经验写成 94 份「给 AI 的专业手册」（Skills），通过 **Blender MCP** 让 AI 直接在 Blender 里建模、贴材质、打灯、导出，交付的是能打开的资产，不是一堆要自己排错的脚本。

支持 Cursor、Claude Code、Kiro、Codex；MIT 协议，可免费商用。

> 核实（2026-11-18 GitHub API）：仓库 2026-07-05 创建，MIT，**约 4293 star**（原文写 830+，应是写稿时的数字），最近推送 2026-11-06。仓库描述：*94 specialist Blender skills for Cursor, Claude Code, Kiro & Codex → MCP-powered production pipeline from blockout to engine export.*

## 94 个技能的覆盖范围

| 类别 | 数量 | 内容 |
|------|------|------|
| 建模 | 9 | 通用、硬表面（武器/机械/工业）、道具、载具、环境、植被、角色、生物、角色原型 |
| 制作流程 | 8 | 雕刻、重拓扑、UV、材质、贴图、毛发梳理、布料模拟、材质开发 |
| 技术 | 9 | 几何节点、灯光、摄影机、渲染、合成、动画、绑定、特效、物理模拟 |
| 管线 | 11 | 程序化建模、场景组装、资产优化、LOD、碰撞体、Unity / Unreal / Godot 专用导出 |

三个引擎导出是**独立技能**：各引擎的坐标轴向、材质规范、碰撞体命名不同，这是「game-ready」和「能看不能用」的区别。

其余技能主要是风格包、类型指引等（见下）。

## blender-director：总指挥

不用自己挑技能。`blender-director` 分析需求，自动把技能串成流水线：

- 「给 Unreal 做把科幻步枪，3000 三角面，写实 PBR」→ 硬表面 → UV → 材质 → 优化 → 导出
- 「做一条寂静岭那种模块化恐怖走廊」→ 恐怖风格 → 环境美术 → 灯光 → 导出
- 「这个生物雕塑重拓扑，1.5 万面预算」→ 重拓扑 → UV → 贴图 → 绑定

内置成品工作流：游戏角色从雕刻到绑定动画 9 个环节、恐怖环境、模块化套件、参考图匹配。

**参考图匹配**：贴概念图 → 分析构图、相机匹配 → 分层建几何、上材质、打灯 → 「截图对比循环」反复渲染比对，直到接近参考图。

## 生产规范

- **命名约定**：网格 `SM_`、材质 `MAT_`、碰撞体 `UCX_`（Unreal 习惯）
- **面数预算**：每类资产有参考预算表，导出前按目标面数卡
- **导出前 QA**：资产优化和检查清单是强制关卡

## 风格与类型库

- **风格包**：lowpoly、像素、手绘、体素、写实，加漫画、黑色电影、HD-2D、8-bit/16-bit 复古、定格动画等十几种
- **恐怖包**：8 个子类型，包括宇宙克苏鲁、身体恐怖、模拟录像带、阈限空间（Backrooms）、民俗恐怖、梦境怪核等
- **13 个游戏品类美术指导**：RPG、生存、潜行、解谜平台、银河恶魔城、类魂、策略模拟、卡牌休闲、开放世界等，每个带「可读性规则」（如类魂用雾门做地标、解谜平台用颜色引导视线）
- **主机时代包**：PS1–PS5 画质演变、CRT 扫描线、VHS 噪点、胶片颗粒

## 安装

接入 Blender MCP 四步：装 `uv` → 装 BlenderMCP 插件 → Blender 侧边栏点连接 → 重启 AI 工具。

Claude Code：

```bash
git clone https://github.com/arjun988/blender-skills.git
cp -r blender-skills/.claude/skills /path/to/your/project/.claude/skills/
```

Cursor / Kiro / Codex 见仓库 README 和官网。Marketplace 审核未过，目前只能从 GitHub 装，更新需手动拉取。

## 局限（原文自述）

- 复杂资产的网格细节仍需人工把关，执行质量强依赖 Blender MCP
- 需求越专业产出越靠谱：「好看的场景」vs「3k 面模块化科幻走廊，PBR，Unreal 导出」是两种结果

## 相关笔记

- [Blender 四个 AI 插件：LoopCut、Scenario、video-to-blender、Blender Skills](2026-10-23-blender-ai-plugins-loopcut-video-to-blender-skills.md)（那篇的 blender-skills 是 EL4CTEO 的 22 技能版本，与本篇不是同一个项目）
- [Codex + Blender 做 IP 角色建模像提线木偶，换付费 Tripo 才像](2026-10-23-codex-blender-vs-tripo-ip-character.md)：实测提醒，AI+Blender 做高还原角色仍弱
- [Scenario for Blender](2026-10-23-scenario-for-blender-ai-plugin-mcp.md)
