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

---

## 补充（2026-11-25）：DrinkCode 的通读笔记

来源：公众号「DrinkCode」《OpenMontage：全球第一个开源的 Agentic视频制作系统》（2026-11-21，https://mp.weixin.qq.com/s/vjPpDngUBihENrQLYfUnxA ）。内容与本笔记高度重合，以下只记新增或更具体的部分。

### 新增细节

- **Star 数**：约 8 万（本笔记 10-28 记的是 7.3 万），仍为 AGPL-3.0，发布时间同为 2026-03 底
- **仓库地址有两个都能打开**：`OpenMontageInc/OpenMontage`（该文给的）和 `calesthio/OpenMontage`（本笔记原记的），两个 URL 实测均返回 200，可能是组织改名后保留了跳转，以仓库内 README 为准
- **三层架构的说法更清楚**：YAML 流水线清单 = 剧本；700+ Markdown 导演技能 = 技能手册；100+ Python 工具 = 动手层。AI 编程助手本身充当编排器，仓库里没有编排器代码
- **钱包保护的具体阈值**：单笔超过 **$0.50 暂停等审批**，默认总预算上限 **$10**
- **防「会动的 PPT」质量门的判定方式**：渲染前校验交付承诺，若声明以运动为主、实际 80% 镜头是静态图，直接拦截不许渲染
- **渲染后自检的具体动作**：ffprobe 验参数 + 在四个位置抽帧查黑屏 + 分析音频静音与爆音
- **`make demo`**：一个 Key 都不配也能立刻渲染一条免费演示视频
- **更多官方示例成本**：《糖果乐园的午后》$0.15（12 张 AI 图加镜头运动与粒子）、《潜入深渊》$0.15（全程零视频生成 API）、《VOID》$0.69（只用一个 OpenAI Key）、《最后的香蕉》$1.33（与本笔记一致）

### 该文列出的局限（值得单独记）

- **离不开 AI 编程助手**：本质是指令系统不是独立应用，没有 Claude Code / Cursor 这类工具跑不起完整流程
- **没有传统剪辑界面**：全程对话式，Backlot 只能围观进度，不能像剪映那样拖时间线
- **Windows 兼容性一般**，部分流水线仍挂 beta，文档自承有粗糙边缘
- **真正的动态视频生成仍依赖付费 API**（Kling、Veo、Runway），免费路径主要是图片动画 + 真实素材剪辑
- **AGPL-3.0 有传染性**，公司内部使用前建议先过法务

### 点评

- 这篇基本是一篇质量不错的通读整理，没有实测，也没有本笔记已有信息之外的独立验证
- 最有价值的新增是两组数字：预算阈值（$0.50 / $10）和那几条更便宜的示例成本；以及把局限集中列了出来——尤其「免费路径做不了真正的动态生成」这条，和本笔记原来的判断一致

---

## 补充（2026-11-27）：AI工具匠的完整教程

来源：公众号「AI工具匠」《69k Star：全球首个开源智能体视频制作系统——OpenMontage 完整教程》（https://mp.weixin.qq.com/s/yYq2lxDvSEYTqVOK7vjVaA ）。主体与本笔记重合，以下只记新增部分。

### 仓库现状核对（2026-11-27，GitHub API）

- `calesthio/OpenMontage` 已**永久重定向**到 `OpenMontageInc/OpenMontage`——解决了 11-25 补充里「两个地址都能打开」的疑问：组织改名，旧地址保留跳转
- Star **83,698**，许可 AGPL-3.0，最近 push 2026-11-16
- 标题里的「69k Star」（正文写 68,996）已过时，原文应是较早写成或照搬旧数据

### 新增细节

- **Backlot 看板命令**：`python -m backlot open` 打开全部项目看板，`python -m backlot open <project-id>` 看单个制作；跑完可点 **▶ REPLAY RUN** 从头回放整条生产过程
- **Windows 无 make 的手动安装**：`py -3 -m venv .venv` → 激活 `.venv\Scripts\Activate.ps1` → `pip install -r requirements.txt` → `cd remotion-composer; npm install` → `pip install piper-tts` → 复制 `.env.example` 为 `.env`
- **本地 GPU 生成**：`make install-gpu`，并设 `VIDEO_GEN_LOCAL_ENABLED=true`
- **可选 Key 清单**：FAL / Kling / Pexels / Suno / ElevenLabs / OpenAI / xAI / Google / ARK（火山方舟）/ HeyGen / Runway，全部可选
- **多助手共用同一契约**：仓库里分别有 `CLAUDE.md`、`CURSOR.md`、`CODEX.md`、`COPILOT.md`，都指向同一份 `AGENT_GUIDE.md`
- **流水线多一条**：除本笔记表中 11 条外，该文还列了「角色动画」
- **更多官方成品**：科幻预告片《SIGNAL FROM TOMORROW》、Blender 3D 展示《OBJECTS IN OVERDRIVE》（每支都附 prompt、流水线、工具和成本，可复现）
- **示例提示词**（真实素材路线）：「做一支 75 秒的雨夜城市生活纪录片，只用真实素材、不要旁白、挽歌氛围、配乐」

### 点评

- 教程型搬运，无实测；有用的是 Backlot 命令、Windows 安装和本地 GPU 开关这几条可操作信息
- 本库 OpenMontage 已有多篇：[08-10 K的算法笔记本](2026-08-10-openmontage-agentic-video.md)、[09-08 视频工作室](2026-09-08-openmontage-ai-video-studio.md)、本篇（主笔记）及 [HPC 整合方案](2026-10-28-hpc-local-video-taishang-openmontage-plan.md)；之后同主题文章继续补到本篇，不再新建
