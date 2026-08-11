# StorySmith AI：9 Agent协作的互动短剧工厂

> 来源：微信公众号文章 · 2026-08-11 · [原文链接](https://mp.weixin.qq.com/s/pEVLpg1qH2J4RE3pqm_7tQ)

## 概述

StorySmith AI 是一个 AI Agent 团队系统，用于生成**互动式分集动画短视频**。观众看完一集后投票决定下一集走向，9个 AI Agent 协作完成从剧本到成片的全流程。

- **项目地址**：github.com/chonlim92/agentic-ai-interactive-short-videos
- **许可证**：CC BY-NC 4.0（非商用，适合学习研究）
- **技术栈**：Next.js 14 + Python + 6种视频模型

## 9 Agent 团队分工

| Agent | 角色 | 阶段 | 模型 |
|-------|------|------|------|
| @showrunner | 统筹/制片人 | 全阶段 | — |
| @writer | 编剧 | 1.剧本 | Claude / GPT-4 |
| @director | 导演 | 2.分镜 | Claude / GPT-4 |
| @character-designer | 角色设计 | 3.角色 | Claude + 图像生成 |
| @artist | 美术/视频生成 | 4.视频 | Seedance 2.0 等 |
| @sound-designer | 音效师 | 6.音频 | MusicGen, Bark TTS |
| @editor | 剪辑师 | 7.合成 | OpenCV, MoviePy |
| @publisher | 发行 | 8.发布 | Claude / GPT-4 |
| @community-manager | 社区运营 | 投票统计 | — |

Showrunner 统筹负责伦理审查和最终复核。每个 Agent 职责边界清晰，某一步出问题只需重跑那一步。

## 8阶段流水线

```
社区运营(收集投票) → 编剧(写剧本) → 统筹(伦理审查) → 导演(拆分镜) →
角色设计(生成参考图) → 美术(生成视频片段) → 质检(4级验证) →
音效师(加音乐旁白) → 剪辑师(合成成片) → 统筹(最终复核) →
发行(部署网站) → 社区运营(监控评论) → 下一轮投票
```

形成"播出→投票→创作→播出"闭环。

## 4级质检体系

| 级别 | 检查内容 | 阈值 |
|------|----------|------|
| Clip 片段级 | 时长/帧率/分辨率/黑帧/静态帧/物体一致性 | 2.5-12s, ≥20fps, ≥480p, <15%黑帧 |
| Consistency 一致性 | 色彩漂移/亮度漂移/SSIM连续性 | <20%色彩, <15%亮度, ≥0.70 SSIM |
| Scene 场景级 | 片段数/时长容差 | ≥2片段, ±25%时长 |
| Episode 整集级 | 总时长/场景数/音频/内容策略 | 150-210s, ≥6场景 |

**失败处理**：片段失败→AI自动生成改进建议→重生对比；3次失败→回导演阶段重做分镜；整集失败→回剪辑阶段重合成。

## 6种视频模型

| 模型 | 来源 | 时长 | 云端 | 本地GPU | 默认 |
|------|------|------|------|---------|------|
| Seedance 2.0 | BytePlus Ark | 5-10s | ✅ | ❌ | ✓ |
| CogVideoX-5B | HuggingFace | 3-6s | ✅ | ✅ | |
| Wan2.1-T2V-14B | HuggingFace | 3-6s | ✅ | ✅ | |
| HunyuanVideo | HuggingFace | 3-6s | ✅ | ✅ | |
| AnimateDiff-Lightning | 本地 | 3-6s | ❌ | ✅ | |
| T2V-1.7B | HuggingFace | 2-4s | ✅ | ✅ | |

视频规格：720×1280（9:16竖屏）、24fps、H264、8Mbps，对标抖音/快手。

## 技术架构

- **双层 Agent**：Chat Agents（VS Code Copilot 交互式创意决策）+ Python Agents（可执行脚本，API调用/视频处理/部署）
- **前端**：Next.js 14, React 18, Tailwind, Framer Motion, Recharts
- **状态管理**：MCP server
- **数据存储**：JSON 文件（无数据库，部署门槛低）
- **校验**：Pydantic
- **测试**：Pytest

## 三种使用方式

1. **Admin 面板**（推荐）：可视化操作，创建故事→创建剧集→运行流水线→查看
2. **CLI 命令**：`python agents/generate_episode.py --story my-story --episode 1`
3. **VS Code Copilot 对话**：`@showrunner generate episode 2 for "the-ancient-without-a-plug"`

## 适合谁用

- 互动短剧创作者（想做"观众投票决定剧情"的内容）
- AI Agent 研究者（多 Agent 协作架构）
- 全栈开发者（Next.js + Python + AI 集成完整案例）
- 影视团队技术负责人（AI化影视生产流程）
- 视频平台开发者（加互动短剧功能）

## 核心价值

传统影视剧组工作流完整映射到 AI Agent 架构：9个 Agent 各司其职，8阶段流水线环环相扣，4级质检保证下限，6种模型灵活切换，观众投票形成闭环。
