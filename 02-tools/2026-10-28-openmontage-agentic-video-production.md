# OpenMontage：开源 Agent 视频制作系统，把 AI 编程助手变成视频制作团队

- **日期**：2026-10-28（收藏）
- **分类**：02 工具与产品
- **仓库**：https://github.com/calesthio/OpenMontage
- **官网**：https://openmontage.video ｜ YouTube：@OpenMontage（每条视频都公开完整 prompt、流程、工具和成本）
- **状态（2026-10-28 查）**：2026-03-29 开源，约 7.3 万 star，曾登 GitHub Trending 日榜第一，最近更新 2026-10-24
- **许可**：AGPL-3.0

> 🔗 **与「太上老君炼丹炉」正好互补**：[炼丹炉](2026-10-28-taishang-laojun-short-video-skill.md)管**内容层**，负责找对标、拆爆款、选题、写口播稿、起标题，回答「拍什么、怎么说」；OpenMontage 管**制作层**，负责调研、分镜、生成或找素材、配音配乐、剪辑、渲染，回答「怎么把视频做出来」。可以先用炼丹炉定选题和逐字稿，再交给 OpenMontage 做成片。两者都能装进 Claude Code、Codex、Cursor。

> 🖥️ **HPC 本地整合方案**：见 [在 McCleary 上整合炼丹炉 + OpenMontage（待执行）](2026-10-28-hpc-local-video-taishang-openmontage-plan.md)

## 一句话

你用大白话描述想要的视频，AI 助手就会按一套完整的制作流程，自己调研、写脚本、生成或检索素材、配旁白和音乐、加字幕、剪辑并渲染出成片。

## 架构：AI 助手本身就是导演

- **没有代码层的调度程序**，你用的 Claude Code、Cursor、Copilot、Windsurf、Codex 就是总指挥
- 统一流程：`research → proposal → script → scene_plan → assets → edit → compose`
- 每条流程有一份 YAML 清单（`pipeline_defs/`），写明阶段、工具、审查标准和通过门槛；每个阶段有一份 Markdown **导演技能**（`skills/pipelines/`），教 AI 这一步怎么做
- AI 调用 Python 工具时，会用打分机制挑供应商：任务匹配、质量、可控性、可靠性、成本、延迟、前后一致性共 7 个维度，并留下可审计的决策日志
- 每个阶段都会自审，状态存成 JSON 检查点（可续跑，含成本快照），创意决定都要人工确认
- **Backlot 实时看板**：本地看板随流程自动填充，显示阶段进度、剧本页、生成中的场景卡、每次选了哪家供应商、花了多少钱

## 规模

- 12 条制作流程、100 多个工具、60 多家供应商接入、700 多份技能与制作知识文件

## 制作流程

| 流程 | 产出 |
|---|---|
| Animated Explainer | 带调研、旁白、配图、音乐的 AI 讲解视频 |
| Animation | 动态图形、动态文字排版 |
| Avatar Spokesperson | 数字人播报 |
| Cinematic | 预告片、品牌片、氛围剪辑 |
| Clip Factory | 从一条长视频切出一批短视频并排序 |
| Documentary Montage | 从免费素材库按语义检索真实镜头，剪成纪录片式拼剪 |
| Hybrid | 原始素材 + AI 生成的辅助画面 |
| Localization & Dub | 字幕、配音、翻译 |
| Podcast Repurpose | 播客转视频 |
| Screen Demo | 软件录屏演示 |
| Talking Head | 口播、访谈剪辑 |

## 特色

- **从参考视频出发**：贴一条 YouTube、Shorts、Reels、TikTok 或本地视频，AI 会分析字幕、节奏、镜头、关键帧和风格，给出 2–3 个差异化方案，写清保留什么、改什么、要花多少钱、用你现有的工具做出来大概什么样，正式开工前还会先出一段样片
- **不花钱也能做真视频**：从 Pexels、Archive.org、NASA、Wikimedia、Unsplash 建素材库，用 CLIP 语义检索真实动态镜头再剪辑，不只是让几张静图动起来
- **调研是正式环节**：写脚本前先做 15–25 次以上的网络搜索，覆盖 YouTube、Reddit、HN、新闻和学术来源，整理成带引用的调研简报
- **质量门槛**：防止成片看起来像幻灯片；合成前先校验方案；渲染后必须自审，包括 ffprobe 检查、抽帧、音频分析、字幕检查
- **预算管控**：执行前先估算成本，可设花费上限和单步审批阈值

## 工具栈

- 图片和视频生成：通过 fal.ai 调用 FLUX、Veo、Kling、MiniMax、Recraft 等
- 配音：Google Chirp3-HD，本地可用 piper-tts
- 渲染合成：Remotion、HyperFrames、Blender（Eevee/Cycles）、FFmpeg
- 每种能力都同时支持本地开源方案和云端 API，没有供应商锁定

## 官方示例成本

- 《THE LAST BANANA》：60 秒 Pixar 风格动画短片，6 段 Kling v3 镜头加旁白、配乐和逐词字幕，总成本 **1.33 美元**
- 《Reimagine Your Universe》：50 秒竖屏，约 4 美元
- 《Imagine the Possibilities》：7 个生成场景合集，素材生成约 5 美元
- 另有科幻预告片（Veo）、Blender 物理动画、纪录片《How Salt Made History》、一句 prompt 生成的 3D 世界等

## 安装

```bash
git clone https://github.com/calesthio/OpenMontage.git
cd OpenMontage
make setup
```

- 依赖：Python 3.10+、FFmpeg、Node.js 18+，外加一个 AI 编程助手
- `.env` 里的 API key 全部可选（如 `FAL_KEY`），配得越多能用的工具越多
- 然后对助手说，例如「Make a 60-second animated explainer about how neural networks learn」
- README 专门有一节写给 OpenClaw 类 Agent：先读 `AGENT_GUIDE.md` 和 `PROJECT_CONTEXT.md`，不要自己即兴编排流程，用 `tools.tool_registry` 查当前能力范围，把每个视频需求当成「先选对流程」的问题

## 个人评价

- **适用**：想批量做讲解、科普、纪录片、短视频的个人或小团队；也适合用来学「用清单 + 技能文件让 Agent 跑长流程」的工程写法
- **注意**：AGPL-3.0 有传染性，改了代码再做成网络服务就要开源；生成式视频要按量付费，免费路线主要靠素材库拼剪和本地 TTS；在 HPC 上跑渲染要另配 Node、FFmpeg 和 Chromium 环境
- **组合用法**：选题和逐字稿用[太上老君炼丹炉](2026-10-28-taishang-laojun-short-video-skill.md)，成片制作用 OpenMontage
