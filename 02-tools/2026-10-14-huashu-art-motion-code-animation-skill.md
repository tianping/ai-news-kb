# huashu-art-motion：用代码画动画，35 种艺术风格 + 9 种解说语法（花叔开源 Skill 实测）

- **日期**：2026-10-14
- **来源**：微信公众号「硅基世纪」
- **分类**：02 工具与产品（视频 / Agent Skills）
- **原文**：https://mp.weixin.qq.com/s/lx6WgODaBjdKoyiDfNHLbQ
- **仓库**：https://github.com/alchaincyf/huashu-art-motion （MIT；作者花叔 @AlchainHust；10-06 创建，10-14 约 3,866 Star / 422 Fork）

## 概述

做讲解短视频除了学 AE / 剪映、或让 AI 视频模型直接生成（常出乱码"伪字"），第三条路是**让 AI 用代码把动画写出来**。huashu-art-motion 是给 Claude Code、Codex 等编程智能体用的 Skill：智能体按需求写 **Canvas 绘图代码或 JSON 分镜** → 无头 Chromium 逐帧调用渲染函数 → ffmpeg 输出 MP4，全程无需图形界面。

## 优点

1. **代码驱动**：字是真字体渲染，中英文不乱码；改文案只需改 JSON 里一个值重渲；同一分镜每次渲染结果一致，可复现
2. **35 种艺术风格**：按年代排开，从洞穴壁画、古埃及、浮世绘到梵高、蒙德里安、包豪斯、8-bit、蒸汽波、2026 扁平插画。示范片含吉卜力水彩风《午后的光与一只猫》、莫奈《日本桥》、超级马里奥式像素冒险片，以及"长卷穿越"骨架（人物从左走到右，跨过交界时世界和他自己的画风一起换）
3. **9 种解说动画语法**：Kurzgesagt 风、Vox 风（剪报拼贴、红线、荧光笔）、白板手绘、3Blue1Brown 风、Storytime、动态文字（Kinetic Type：大字砸入、弹簧过冲、色带斜切转场）、发布会 UI、财经图表、主播式财经解说。每种附"语法卡"，写明配色、节奏、镜头运动的具体数字
4. **JSON 参数化片段（8 种）**：只写画布尺寸、总时长、各时间点出现什么（标题/要点/数字/高亮），**渲染时长严格等于所写时长**——可先配音拿每句起止时间，再填进 JSON，音画天然对齐
5. **无需显卡**：只依赖 uv、ffmpeg、Playwright Chromium，不需 GPU 和 Node 构建
6. **审美参数来自拆解真实作品**：配色比例、线宽、写字速度、转场方式

## 安装

```bash
npx skills add alchaincyf/huashu-art-motion
uv run --with playwright playwright install chromium   # 首次运行前装浏览器
```

然后对智能体说"用 huashu-art-motion 做一段竖屏动态文字动画，内容是……"。Skill 文档基本是中文。

## 作者实测（无 GPU 的 Linux 服务器，未改代码即跑通）

| 片段 | 渲染耗时 |
|---|---|
| 19 秒 1080×1920 30fps 动态文字片 | 约 15–20 秒 |
| 7.5 秒竖屏动态文字小样 | 约 8 秒 |
| 梵高风艺术场景 4 秒 | 约 75 秒 |

流程：写 7 句口播 → 开源 TTS 配音拿每句时长 → 时间点填进 JSON → 一条命令渲染 MP4 → ffmpeg 混入配音和音乐。基础版约 1.7 分钟出片，加卡点、音效、推镜震屏约 3 分钟；大部分时间花在反复调整上。

## 坑

- **白板风在竖屏上偏素**（字小留白多），换动态文字效果好得多——选语法比调参数更重要
- **平台安全区**：动态文字默认大字撑满约 86% 画宽，左右边距设置不生效，最右侧会被抖音/TikTok 的点赞评论按钮挡住；作者改了一行代码让字宽跟随边距
- **中文配音要自己解决**：自带口播流程默认接火山引擎 TTS（付费、需凭证），Linux 无系统语音后端；开源 Kokoro 中文可用但个别词会读错，需改写文案绕开
- **单段建议 ≤ 8 秒**，超过有警告，长片拆段再拼
- **艺术风格场景复用性一般**：每换主题都要写新场景代码、渲染慢；解说类优先用 JSON 片段
- **字体子集**：中文黑体只含 GB2312 的 6763 个常用字，生僻字可能缺字，用仓库脚本补字
- 授权：代码与文档 MIT；仓库里的花叔形象素材仅供演示；内置字体各自保留 SIL OFL 协议

## 备注

- 与本库 [HyperFrames](2026-10-14-hyperframes-html-to-video-render-engine.md) 同属"视频产物是代码"路线（无头 Chrome 逐帧 + FFmpeg）：HyperFrames 是通用 HTML 渲染引擎，huashu-art-motion 侧重内置画风与解说语法的审美预设。
- 本库 [Claude Opus 5.5 视频教程：万能提示词 + GitHub 项目](2026-10-08-opus55-video-universal-prompt-github-projects.md) 曾简要列出此项目；同作者另有 [huashu mac-use](2026-09-07-huashu-mac-use-agent-computer-control.md)。
