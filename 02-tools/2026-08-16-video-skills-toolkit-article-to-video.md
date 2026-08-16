# 实测 bozhouDev/video-skills-toolkit：把文章变成视频，先把声音钉在时间线上

> 来源：[实测 bozhouDev/video-skills-toolkit](https://mp.weixin.qq.com/s/ucoefw7p60oerRcVMYSCoQ) · 2026-08-16
> GitHub：https://github.com/bozhouDev/video-skills-toolkit

---

## 概览

**video-skills-toolkit** 是一套面向自媒体视频创作的 Agent Skills 工具包，作者 bozhouDev。核心思路：**先把声音钉在时间线上，再围绕声音做画面**——配音定稿后才做分镜、口播成片、BGM 和封面。

- MIT 协议开源
- 10 个核心 Skill + 3 个随包依赖
- 适配 Claude Code 等 Agent 平台

---

## 核心流水线

```
调研：viral-video-benchmark
  └─ media-to-transcript → 最终逐字稿/关键帧 → 爆款证据与八部分拆解
  └─ content-remix → 二创迁移卡 → 已确认创作逐字稿

制作：已确认创作逐字稿
  → minimax-voice-director    （配音）
  → audio-to-subtitles         （字幕对齐）
  → video-script               （导演稿）
  → talking-head-hyperframes   （口播舞台）
  → hyperframes-scene-animator （镜头实现）
  → music（BGM）
  → douyin-cover               （封面）
```

**关键边界**：正式配音前必须有已确认的逐字稿，本工具包不把"边写稿边做分镜"当作正式流程。

---

## 10 个核心 Skill

| Skill | 职责 |
|-------|------|
| **viral-video-benchmark** | 扫描抖音/小红书账号近期作品，用确定性规则判断爆款，建立证据包，拆解并归档 |
| **media-to-transcript** | 下载平台视频，抽取切分音频，通过火山引擎 AUC ASR 产出原始转写、纠错提示和最终逐字稿 |
| **content-remix** | 把已拆解的爆款机制迁移成自己的小红书图文、抖音口播或小红书视频内部草稿 |
| **minimax-voice-director** | 为锁定逐字稿制作可审批的 MiniMax 声音导演稿，生成、选 Take 并发布最终人声 |
| **audio-to-subtitles** | 把最终音频/视频转成 SRT、VTT 和对齐 JSON；保留轻量 MiniMax TTS 兼容入口 |
| **video-script** | 使用锁定音频和字幕生成管线无关的导演契约、Beat Graph、制作规格和 motion contract |
| **talking-head-hyperframes** | 生成并验证 HyperFrames 口播项目的固定舞台、PIP、输入归档、manifest 与执行交接 |
| **hyperframes-scene-animator** | 实现逐镜静态页、语义动效、转场、声音 cue、proof 和预览审查闭环 |
| **music** | 通过 ElevenLabs Music API 生成 BGM，优先复用已登记音轨，管理哈希和审批状态 |
| **douyin-cover** | 诊断、生成和改版抖音/视频号/小红书等平台的短视频封面 |

### 3 个随包依赖（非创作核心）

| Skill | 用途 |
|-------|------|
| video-transcript | 已废弃转写入口；仅保留 B站/抖音/小红书/YouTube 下载 helper |
| hook-writing | 提供钩子类型、情绪机制和只读历史钩子库 |
| obsidian-bases | 维护 Obsidian .base 视图、过滤和公式 |

---

## 关键设计原则

| 原则 | 说明 |
|------|------|
| **声音先行** | 逐字稿确认 → 配音定稿 → 字幕对齐 → 导演稿 → 口播成片，声音钉在时间线上再做画面 |
| **不自动发布** | content-remix 生成迁移方案或内部草稿，不自动发布 |
| **配音前必须有定稿** | 不把"边写稿边做分镜"当正式流程 |
| **导演与执行分离** | video-script 只做导演规划不写场景代码；talking-head-hyperframes 只建舞台，镜头实现归 hyperframes-scene-animator |
| **音轨复用优先** | music 只在已登记音轨不适用时消耗 ElevenLabs 额度 |
| **封面最后做** | douyin-cover 在标题与发布方向锁定后执行，可与成片收尾并行 |

---

## 环境要求

| 模块 | 依赖 |
|------|------|
| HyperFrames | Node.js, npx, FFmpeg/FFprobe, Chrome/Chromium；模板固定 hyperframes@0.7.65 |
| MiniMax voice | Python 3, requests, PyYAML, FFmpeg/FFprobe, MINIMAX_API_KEY, MINIMAX_VOICE_ID |
| Subtitles | Bun/Node.js, MediaKit API 凭证；本地文件上传还需 Cloudflare R2 凭证 |
| Media transcript | Python 3, FFmpeg/FFprobe, Playwright + Chromium；YouTube 需 yt-dlp；AUC 需 VOLCENGINE_SPEECH_API_KEY |
| Viral benchmark | 可读取已登录抖音/小红书页面的浏览器自动化能力 |
| BGM generation | ElevenLabs 付费方案与 ELEVENLABS_API_KEY |
| Cover generation | 当前 Agent 环境中的 imagegen / image_gen 能力 |

---

## 安装方式

```bash
git clone https://github.com/bozhouDev/video-skills-toolkit.git
cd video-skills-toolkit

# 复制 skills/ 到 Agent Skill 根目录
mkdir -p ~/.agents/skills
cp -R skills/* ~/.agents/skills/
```

也可放入项目级 `.agents/skills/`、`.claude/skills/` 或当前 Agent 平台的对应 Skill 目录。**不要只复制单个 SKILL.md**，脚本、references、assets 和测试夹具都是包的一部分。

---

## 与旧版区别

- 旧版包含 talking-head-remotion 和 sketch-story-remotion，新版已移除
- 旧版被评价"PPT 演播感"过强，新版基于 HyperFrames 重构
- 新版围绕"爆款调研与转写 → 内容二创 → 配音与字幕 → 导演稿 → HyperFrames 口播成片 → BGM 与抖音封面"重新组织

---

## 核心价值

> **"把文章变成视频"的关键不是堆画面，而是先把声音钉在时间线上。**
>
> 声音定稿 → 字幕对齐 → 导演规划 → 口播成片 → 镜头实现 → BGM → 封面
>
> 每一步都有明确的输入和输出，前一步没锁定不进入下一步。这不是"AI 一键生成视频"，而是"AI 辅助每一步都可控的视频生产流水线"。