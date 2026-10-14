# HyperFrames：写 HTML 就能出视频，HeyGen 开源渲染引擎（不用显卡、无渲染费）

- **日期**：2026-10-14
- **来源**：微信公众号「大刘的AGI」
- **分类**：02 工具与产品（视频 / Agent Skills）
- **原文**：https://mp.weixin.qq.com/s/oGsEuwcTaGzpIG6EzF4Plg
- **仓库**：https://github.com/heygen-com/hyperframes （Apache-2.0，TypeScript）

## 概述

HeyGen 开源的 **HyperFrames** 是**渲染引擎，不是生成模型**：画面由你或 Agent 用 HTML/CSS/图片/视频/字体写出来，它用**无头 Chrome 逐帧 seek 截图 + FFmpeg 编码**，同一份输入永远产出同一支视频。不需要 GPU、模型或 API key，本地渲染无费用。

**时间线澄清**：仓库 2026-03-10 创建，03-25 发首版，现已到 0.8.149——不是"本周新开源"，而是本周因 Agent 技能和插件集成被大量转发。作者 10-12 实查：约 6.1 万 star、5,454 fork、260 open issue；npm 包 `hyperframes` 上周下载 75 万次；10-11 一天发四个版本。

## 核心概念：composition 就是一个 HTML 文件

没有专有时间轴格式，DOM 上用 `data-*` 声明时间、时长、轨道：

```html
<div id="stage" data-composition-id="launch" data-start="0" data-width="1920" data-height="1080">
  <video class="clip" data-start="0" data-duration="6" data-track-index="0" src="intro.mp4" muted playsinline></video>
  <h1 id="title" class="clip" data-start="1" data-duration="4" data-track-index="1">Launch day</h1>
  <audio data-start="0" data-duration="6" data-track-index="2" data-volume="0.5" src="music.wav"></audio>
  <script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js"></script>
  <script>
    const tl = gsap.timeline({ paused: true });
    tl.from("#title", { opacity: 0, y: 40, duration: 0.8 }, 1);
    window.__timelines = window.__timelines || {};
    window.__timelines.launch = tl;
  </script>
</div>
```

- **时间写在 DOM 上**：`data-start` / `data-duration` / `data-track-index` = 开始、时长、轨道
- **动画必须可寻址（seekable）**：`paused: true` + 时间轴，渲染器把每帧"拨"到指定时刻，不依赖真实时钟。支持 GSAP、CSS 动画、Lottie、Three.js、Anime.js、WAAPI，可自写适配器
- **无构建步骤**：`index.html` 直接在浏览器里播，预览不用打包——与 Remotion 的分水岭

## 21 个 Agent 技能

- **路由技能 `/hyperframes`**：意图面试 → 路由表 → brief / storyboard / 审片 / 生产 / 帧 worker 契约文件；每个流程配小文件写清输入输出和触发条件。**先判断再懒加载**，不把 21 个技能全塞进上下文
- **十个创作流程**：`/product-launch-video`（给网址做发布片，30–90 秒最佳，最长约 3 分钟）、`/faceless-explainer`（纯概念讲解）、`/pr-to-video`（用 gh CLI 把 PR 讲成变更说明）、`/embedded-captions`（给已有口播加字幕）、`/talking-head-recut`（访谈/播客图形包装）、`/motion-graphics`（10 秒内无解说动效，可做透明叠加）、`/music-to-video`（踩点）、`/slideshow`（交互式 deck）、`/general-video`、`/remotion-to-hyperframes`（单向迁移）
- **八个领域技能**按需加载，最重要的 `/hyperframes-core` 定义 composition 契约（data-* 时间属性、`class="clip"`、轨道、子 composition、变量、媒体播放归属、确定性规则）

## 安装

```bash
# Claude Code
claude plugin marketplace add heygen-com/hyperframes
claude plugin install hyperframes@hyperframes      # 之后用 /hyperframes:hyperframes
# Copilot CLI
copilot plugin marketplace add heygen-com/hyperframes
copilot plugin install hyperframes@hyperframes
# Gemini CLI
gemini extensions install https://github.com/heygen-com/hyperframes --auto-update
# 独立技能（含 OpenCode 等）
npx skills add heygen-com/hyperframes
```

- **别一次装全 21 个**：选 Core Skills 即可，路由技能真需要时再装对应流程；`--all` 才装全集
- 两条更新路径：`skills add` 走 skills.sh 注册表快照，可能滞后 main 几小时；`npx hyperframes skills update` 直接从 main 装
- **Cursor**：官方明说仓库有 manifest ≠ 已上公开 Cursor marketplace，需团队 marketplace 导入或本地加载

## CLI 全流程

```bash
npx hyperframes init       # 初始化（就位核心技能集）
npx hyperframes lint       # 静态检查
npx hyperframes check      # 校验 composition
npx hyperframes preview    # 本地浏览器预览，改 HTML 刷新即可
npx hyperframes render --output final.mp4
npx hyperframes add flash-through-white | instagram-follow | data-chart   # 现成组件
```

| 格式 | 用途 |
|---|---|
| mp4（默认） | 正常交付 |
| mov | 剪辑中间文件，保留 alpha |
| webm | 透明叠加层（网页/直播） |
| gif | 短循环预览，可配 `--fps 15` |
| png | RGBA 帧序列 |
| hls | HLS VOD 流，默认 4 秒分段 |

帧率默认取 `data-fps`，无则 30，`--fps 60` 覆盖；有快检和母版两档质量预设。可本地、Docker 或 **AWS Lambda** 分布式渲染。本地渲染不耗 HeyGen 额度，托管的口语/数字人/生成式素材另算；`npx hyperframes usage --json` 查余量，失败或返回 `status: unknown` 就直说不知道，不要猜。

上手顺序：`init` → 在 Agent 里用 `/hyperframes` 说清素材（网址/PR/文本/既有片）、时长、格式 → `preview` → `lint` + `check` → `render` → **手动看一遍导出文件**。

## 确定性要管理

- 脚手架在 `package.json` **钉住 hyperframes 版本**，不自动升级，保证可复现
- 升级三步：`npx hyperframes@latest upgrade --project . --check`（只读探测）→ `upgrade --project .` → `npx hyperframes check`
- **`check` 通过只证明 composition 在新版合法，不证明渲染与旧版逐帧一致**；升级要在总结里写明升前升后版本；`check` 失败就改回钉住版本并说明原因
- 作者建议（非官方）：把渲染当回归测试——同一 composition 渲染两遍比对，严格时 `--format png` 逐帧比

**渲染失败排查**：`doctor` → `lint` → `check`；保留**第一个**确切错误；预览只证明能播，导出文件才证明交付正确；需 Node 22+。

## 与 Remotion 对比

| | HyperFrames | Remotion |
|---|---|---|
| 写法 | HTML + CSS + 可寻址动画 | React 组件 |
| 构建 | 无，index.html 直接播 | 需要打包器 |
| 交给 Agent | 纯 HTML 文件 | JSX / React 工程 |
| 库时钟动画 | 可寻址，逐帧精确 | 需额外小心 |
| 分布式渲染 | 本地 + AWS Lambda | Remotion Lambda（更成熟） |
| 许可 | **Apache-2.0** | Remotion License（源码可见，商用有条件） |

靠视频交付挣钱的人，**许可那一行比 HTML vs React 更重要**。衍生概念 **frame.md**：把网页设计规范翻译成面向镜头的 DESIGN.md 超集，让 Agent 写视频时不用猜尺度。

## 适用判断

- **适合**：批量同模板视频、渲染进 CI 做回归、让 Agent 代写而非手搓时间轴、品牌规范落到每一帧、不想付按次渲染费
- **先别急**：要生成从未存在的画面（找视频模型）；要开箱即用模板库；十分钟以上长片；团队已深耕 Remotion；想走公开 Cursor marketplace

**作者结论**：被验证的不是"AI 会做视频"，而是"**视频的产物可以是代码**"——能进版本管理、能 review、能被 Agent 批量改写、能在 CI 里跑。

## 备注

- 本库此前多篇笔记提到 HyperFrames 作为下游渲染器：[video-skills-toolkit](2026-08-16-video-skills-toolkit-article-to-video.md)、[Codex 自动剪辑导演 Skill](2026-08-25-codex-auto-editing-director-skill.md)（注意：作者曾因 HyperFrames 同质化模板被限流）、[OpenMontage](2026-09-08-openmontage-ai-video-studio.md)；本篇是 HyperFrames 本身的专门笔记。
- Star、下载量为作者 10-12 实查数据，未独立复核。
