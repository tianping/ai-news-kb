# Kinema：Codex + Claude Code 驱动的 AI 影像智能体，一条主题出成片

**来源：** 微信公众号文章，2026-09-07
**链接：** https://mp.weixin.qq.com/s/4eJxS0QXZArsm7kI1wEVnQ
**原始仓库：** https://gitee.com/smallc/Kinema
**许可证：** AGPL-3.0
**标签：** #AI视频 #AI电影 #智能体 #管线 #开源

---

## 一句话

给一个主题，出一条成片。Kinema 是 BladeX 团队（smallchill）开源的 AI 影像智能体——不是单点模型，而是一整套把「主题」推进到「成片」的制作操作系统。

⭐ 31 / Fork 9（极早期，仓库创建于 2026-09-03）。

---

## 核心理念

把分散在多个工具间的环节接进同一条管线——**改一处设定，下游镜头当场被标为「过期」**，一致性不再靠人脑硬记。

```
Kinema = 多层智能规划 + 资产血缘管理 + 统一制作管线
```

生图 / 配音 / 字幕 / 运镜 / 合成，一条管线贯通。

---

## 三层架构

| 层 | 位置 | 职责 |
|---|---|---|
| 指挥层 | `agent/` | 把自然语言意图翻译成结构化制作计划；`manifest.json`（skill 注册表）、`contracts.json`（PromptSpec / ChapterPlan） |
| 执行引擎 | `engine/kinema/` | 纯 Python，内部无 LLM；100+ 模块，调云端 API 生图/视频/配音/合成；成本可控（--dry-run 逐镜报价，done 镜头锁定） |
| 制作台 | `studio/` | 五道关口可视化确认 + 回滚 |

---

## 三种渲染模式

| 模式 | 画面 | 声音 | 视频成本 | 适用场景 |
|---|---|---|---|---|
| `kenburns` | 静图缓动运镜 | Kinema 配音 + BGM | 零（纯本地） | 零成本出样片过节奏 |
| `dubbed` | Seedance 图生视频、闭唇 | Kinema 配音 + BGM | 按秒计费 | 全旁白解说章 |
| `native` | Seedance 原生音画 | 模型自声上主轨 | 按秒计费 | 有对白要口型同步 |

---

## 快速上手

```bash
brew install ffmpeg                     # macOS；Debian: sudo apt install ffmpeg
cd engine
python3 -m kinema doctor                # 自检 ffmpeg / 配置 / providers / 存储后端
cp examples/sample_project.json /tmp/demo.json
python3 -m kinema run --project /tmp/demo.json --mock   # 离线端到端、零成本
python3 -m kinema studio                # 打开制作台 → http://127.0.0.1:8787
```

普通笔记本即可，纯 CPU，无需显卡。

### 做一集出来的完整命令链

```bash
# 建项目
python3 -m kinema project new --title "剑与雨" --id bladerain --profile cyberpunk
python3 -m kinema chapter new bladerain --title "第四十七层"   # → ch01

# 设定集（一致性根基）
python3 -m kinema project refs bladerain

# 零成本静态体检
python3 -m kinema lint --chapter bladerain/ch01

# 先出首镜定风格
python3 -m kinema gen-image --chapter bladerain/ch01 --only 1

# 配音（按角色卡声线）
python3 -m kinema tts --chapter bladerain/ch01

# 零成本样片过节奏
python3 -m kinema animatic --chapter bladerain/ch01

# 花钱前逐镜报价
python3 -m kinema gen-video --chapter bladerain/ch01 --dry-run

# 只烧点过头的镜（--approved-only）
python3 -m kinema gen-video --chapter bladerain/ch01 --approved-only

# 成片（每道关口先落盘等你确认）
python3 -m kinema assemble --chapter bladerain/ch01
```

---

## 关键设计亮点

**成本控制内建，不是事后补救：**
- `--dry-run` 逐镜报价，花钱前知道总预算
- 标 `done` 的镜头被锁定，`--force` 也不覆盖
- 整批超 budget 时，事前闸不发任何请求

**资产血缘：** 设定图一改，下游镜头自动标「过期」，从设定集而非成片逆向修。

**多宿主兼容：** AGENTS.md 统一适配 Claude Code / Codex / Cursor / Copilot / Windsurf / Aider / Zed。能力包 .claude/skills/ 单源编辑。

---

## 真实能力版图

- 40+ 画风档
- 30+ 运镜预设
- 10+ 特效
- 2000+ 离线测试
- 内置 100+ BGM + 18 音效（CC0），无密钥也能自动降级出片

**已演示的场景：**
- 🎬 一句主题 → 完整 AI 漫剧（文案 → 拆提示词 → 定风格 → 配音 → 样片 → 报价 → 成片）
- 🎞️ 实拍驱动深度捕捉：上传实拍片，CPU 提取深度浮雕 + 骨骼生成控制视频，运动来自实拍，外观来自设定图
- 📚 长篇小说改剧本：十章一批，七项自动复核（设定一致性·人设·情节连贯·AI 腔·文风·伏笔·节奏）

---

## 最佳实践 Checklist

- [ ] 先 lint 再花钱：`kinema lint` 零成本体检，挡掉运镜雷同/AI 腔
- [ ] 首镜定风格：`gen-image --only 1` 锁死画风，再批量出图
- [ ] 固定 seed + 设定图挂载：角色三区两视、道具三视图、场景主视觉逐镜挂载
- [ ] 用预算闸：`--dry-run` 报价 + budget 事前闸，超预算不发请求
- [ ] 逐镜 approve：`--approved-only` 只烧确认过的镜头，拒绝「一键全生成」
- [ ] 本地先 mock：`run --mock` 用占位图+合成音跑通管线每一段
- [ ] 善用血缘回滚：设定图一改，下游镜头标过期，从设定集逆向修

---

## 注意事项

⚠️ **AGPL-3.0 强 copyleft**：个人使用/学习/研究/评估完全免费；对外提供 SaaS、嵌入闭源产品、OEM 交付、或做不开源的内部平台，需购买商业授权（bladex.cn）。

⚠️ **项目极早期**：API 与命令可能快速变动，投产前请自行验证破坏性变更风险。

⚠️ **真实成片要额度**：kenburns 零成本，但 dubbed/native 依赖 Seedream/Seedance/Veo/ElevenLabs 等云端 API，按秒/按张计费；务必先用 `--dry-run` 报价。

🔧 **深度捕捉需额外安装**：实拍驱动功能依赖 ONNX Runtime + OpenCV + RTMPose/Depth Anything V2/MediaPipe，纯 CPU 推理，安装步骤见 SETUP.md。

---

## 资源链接

| 资源 | 链接 |
|---|---|
| Gitee 源码 | gitee.com/smallc/Kinema |
| 项目主页 | bladex.cn |
| 工程指南 | gitee.com/smallc/Kinema/blob/main/AGENTS.md |
| 开发手册 | gitee.com/smallc/Kinema/blob/main/DEVELOP.md |
| 能力包索引 | gitee.com/smallc/Kinema/blob/main/docs/skills/INDEX.md |
