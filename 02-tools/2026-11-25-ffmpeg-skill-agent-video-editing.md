# ffmpeg-skill：给编程智能体一个本地视频剪辑工具箱，42 个工具 + 改完自己复检（KuaiZApp）

- **日期**：2026-11-25（收录）
- **来源**：微信公众号「KuaiZApp」，原标题「推荐一个让 AI 帮你剪视频的开源项目：ffmpeg-skill，改完还能自己验收」
- **原文**：https://mp.weixin.qq.com/s/QMztg-I1km8ACz3hByBBPA
- **项目**：https://github.com/kajisho5/ffmpeg-skill （已核对：自述「给编程智能体一个视频剪辑器，42 个 FFmpeg 工具 + 机器可读合同 + MCP 服务器，无云端、无 API Key、纯 Python 标准库」）
- **许可**：MIT
- **分类**：工具与产品 / Agent Skill / 视频处理
- **相关**：[onetake 一镜到底产品片 Skill](2026-10-21-onetake-one-shot-promo-video-skill.md) · [OpenMontage](2026-10-28-openmontage-agentic-video-production.md) · [Concat 开源剪辑器内置 MCP](2026-11-22-concat-open-source-video-editor-mcp.md)

## 一句话

它解决的不是"剪什么"，而是"让智能体剪得靠谱、并且能证明剪对了"：每个剪辑动作都是带命名参数的脚本，执行后再探一次产物核对时长、尺寸、音轨，把"我觉得剪好了"变成"测得 59.98 秒、1080×1920、30 fps、-14.0 LUFS、VERIFIED"。

## 它是"手"不是"脑"

选哪些片段、什么算好字幕、成片能不能交，这些判断在你或上层智能体，不在这个仓库里。字幕按你给的原文烧录，它不改文案。

## 四个核心抽象（这才是可借鉴的部分）

1. **Probe 先测量**：每个任务从 `probe.py` 开始，读真实时长、帧率、分辨率、色彩、音轨，**禁止靠文件名猜参数**
2. **Typed tools 类型化工具**：每个操作是带 argparse 参数的脚本，调用方只能传命名选项，**不能塞 shell 字符串或任意 filter graph**，减少注入和瞎写参数的空间
3. **Contract 机器可读合同**：`contract --json` 声明每个工具吃什么、写什么、依赖哪些 FFmpeg 组件、怎么验证；MCP 工具列表直接从它生成。输入合同不是手写的，而是**运行时从 CLI 的 argparse 解析器派生**，CI 发现漂移就构建失败
4. **Verify 执行后复检**：探源文件和产物，核对时长尺寸音轨；画面变了还生成缩略图拼图给人看

## 能力清单

探测 / 无损优先的切割拼接 / 按阈值去静音 / 横转竖（crop、pad 多种填充）/ 烧录字幕与逐词高亮卡拉OK / 叠层与动态图形 / HDR 转 SDR 与 LUT 调色 / 降噪与响度归一化到 -14 LUFS / 音画漂移校正与多机位对齐 / 按平台规格核对交付条件输出 JSON / 用 `project.json` 一次渲染整条编辑链并支持批量文件夹 / 生成本地审片 HTML

## 两种入口

| 路径 | 场景 | 机制 |
|---|---|---|
| 命令行独立用 | 快速剪一个文件 | `npx ffmpeg-skill`，直接调本机 ffmpeg/ffprobe |
| 智能体 / MCP | 自然语言连续改片 | Claude Code、Cursor、Codex 读 SKILL.md，经 MCP 调工具 |

## 几个实用设计

- **零云零 Key**：素材留本地，不需要账号；语音转写也可离线（装 faster-whisper / parakeet-mlx / openai-whisper 之一）
- **42 个工具只暴露 12 个**：MCP 默认只向客户端列出 12 个核心工具以节省上下文，设 `FFMPEG_SKILL_MCP_FULL=1` 才列全部，但按名字仍可逐个调用
- **护住源文件**：输出路径与已声明输入相同会被拒绝，直接改文件要显式 `--overwrite`
- **审片页**：本地 HTML，支持中英日韩四语，点片段或字幕编号写要求，一键复制成给智能体的指令，什么都不上传
- **兼容范围**：README 标注 FFmpeg 6.1 / 7.1 / 8 / 9 均在 CI 测过

## 局限（原文列得很实在）

- 不是一体化剪辑软件，**没有时间线拖拽界面**
- 不负责创意判断，不决定剪点，不改写字幕文案
- 字幕转写要另装语音引擎，不装时 `caption.py --transcribe` 直接报错，首次运行还要下模型权重
- `setup` 装 FFmpeg 依赖 brew / winget / apt 之一；三者都没有时会装好技能但提示 FFmpeg 仍缺失并非零退出。还需本机已有 Node 16+ 和 Python 3.10+，它不替你装
- **42 个工具里只有 12 个标稳定**，其余成熟度不一，生产前自己验
- 作者在配套论文里给自己的"验证驱动"纪律打了**部分符合**，不是完全达标
- 官网 showcase 九段演示素材是 AI 生成的（Google Veo），只有三段手机实拍，别拿演示效果当自己素材的预期

## 上手

```bash
npx ffmpeg-skill setup     # 装技能 + 缺 FFmpeg 时打印并执行包管理器命令 + 跑 doctor 体检
npx ffmpeg-skill doctor    # 单独体检 ffmpeg/ffprobe 与依赖组件
npx ffmpeg-skill contract --json | head -40   # 看机器可读合同
npx ffmpeg-skill review    # 生成本地审片页
```
只装技能不碰系统包管理器：用不带 `setup` 的 `npx ffmpeg-skill`，或 `setup --no-ffmpeg`。Cursor 用 `--cursor`、Codex 用 `--codex`、三个都装用 `--all`。

建议路径：先跑 setup 看 doctor 全绿 → 拿不重要的素材试"剪静音 + 转 9:16" → 打开审片页逐步建立信任 → 再上正片。

## 点评

- 这个项目最值得学的其实和视频无关：**「先测量、只给命名参数、合同从代码派生、执行后复检」这四条，是任何让 Agent 操作真实文件的工具都该有的结构**。和库里 iPhone Use 那篇的失败恢复设计、Code2Video 的"渲染后读回自检"是同一类工程直觉
- 和 Concat 的区别：Concat 是带界面的剪辑器顺手内置了 MCP；这个是纯工具箱，没界面但把可验证性做到底
- 原文结尾有作者自家付费软件安装服务的推广，内容本身是客观的项目介绍
- star 数未核；"42 个工具、12 个稳定"等数字来自原文与 README
