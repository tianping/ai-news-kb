# 在 McCleary HPC 上整合「太上老君炼丹炉」+ OpenMontage 本地制作视频（方案，待执行）

- **日期**：2026-10-28
- **分类**：02 工具与产品
- **状态**：⏸️ **只写了方案，尚未执行**，等 Dejian 决定后再开始第一步
- **相关笔记**：[太上老君·AI炼丹炉](2026-10-28-taishang-laojun-short-video-skill.md)（内容层：选题、逐字稿、标题）｜[OpenMontage](2026-10-28-openmontage-agentic-video-production.md)（制作层：分镜、素材、配音、剪辑、渲染）

## 结论

可行。在 HPC 上用 Claude Code 当整合层：炼丹炉负责「拍什么、怎么说」，OpenMontage 负责「怎么做出来」。两者都是给 Claude Code 用的，不需要另写胶水代码。

## 环境现状（2026-10-28 查）

| 项目 | 现状 |
|---|---|
| FFmpeg | 已有（`~/bin/ffmpeg`） |
| Node.js | 已有 v26.5.0，另有模块 `nodejs/20.13.1-GCCcore-13.3.0` |
| Python | 已有 3.10+，可在 env4claude 建 venv |
| GPU | 有 `gpu`、`gpu_devel`、`scavenge_gpu`、`priority_gpu` 分区 |
| 中文字体 | `env4claude/fonts/NotoSansSC-VF.ttf`，可用于中文字幕 |
| Chromium | 之前 Marp 用过 Puppeteer，但 Remotion 用自己的 headless shell，需另测 |
| 存储 | `/gpfs/gibbs` 已用 92%（剩约 30T 共享），本地模型要节制 |

## 整合架构

1. **炼丹炉**：`npx -y skills add liuhaisi2026212-maker/taishang-laojun -g --all`，只有 Markdown 技能文件，没有依赖
2. **OpenMontage**：克隆到 `/gpfs/gibbs/pi/ycga/mane/dz288/home2/env4claude/OpenMontage/`（遵守「Claude 安装的工具放 env4claude，不放 home」的规定）
   - Python venv + `pip install -r requirements.txt`，缓存设到 `env4claude/cache/pip`
   - `remotion-composer/` 执行 `npm install`，`npm_config_cache` 设到 `env4claude/cache/npm`
   - 离线配音用 `piper-tts`，使用中文音色（如 `zh_CN-huayan`）
   - 复制 `.env.example` 为 `.env`，API key 全部可选，先不配
3. **工作流**：炼丹炉出选题、逐字稿、标题 → 交给 OpenMontage 走 `research → proposal → script → scene_plan → assets → edit → compose` → 在 GPU 或 CPU 节点上用 sbatch 渲染

## 分步计划

### 第一步：免费路线跑通（不用 GPU，约 1–2 小时）

- 装好炼丹炉和 OpenMontage 基础环境
- 选一个题目，做一条 30–60 秒的中文讲解视频，素材来自免费素材库和静图动画，配 Piper 中文旁白
- 要验证的内容：
  - [ ] Remotion headless Chromium 在 RHEL 8 上能否启动（缺系统库时考虑 `--browser-executable` 指向已有 Chromium，或换用 Apptainer 容器）
  - [ ] Piper 中文发音质量
  - [ ] 中文字幕字体正常渲染（Noto Sans SC）
  - [ ] 计算节点能否联网（网络调研、素材下载）；不能联网的话，联网步骤放登录节点做，渲染再提交到计算节点
  - [ ] 运行 `registry.support_envelope()` 查当前可用能力

### 第二步：本地生成视频（GPU）

- `make install-gpu`，选**一个**模型：首选 `wan2.2-ti2v-5b`（约需 24 GB 以上显存）；`wan2.1-14b` 要 A100 级显卡
- 模型权重放在 env4claude 下，并设好 HuggingFace 缓存目录，防止下载到 home
- 写成 sbatch 脚本（`--partition=gpu --gpus=1`），在 GPU 节点上生成镜头，再回到合成步骤

## 风险与预期

- **Remotion 的 Chromium 依赖**：最可能卡住的地方
- **存储**：每个本地视频模型几十 GB，只装一两个
- **画质**：本地开源模型明显不如 Kling、Veo。完全免费时最稳的做法是**真实素材 + Piper 配音 + Remotion 动画**，本地生成镜头只做点缀；以后需要高质量镜头，再考虑配 `FAL_KEY` 按量付费（官方示例 60 秒约 1–5 美元）
- **许可**：炼丹炉是 CC BY-NC 4.0（非商用），OpenMontage 是 AGPL-3.0；自用没问题，商用或做成对外服务前要再看许可

## 执行后待补

- 实际安装版本、遇到的问题和解决方法，写回本笔记，并按惯例登记到 `env4claude/README.md`
