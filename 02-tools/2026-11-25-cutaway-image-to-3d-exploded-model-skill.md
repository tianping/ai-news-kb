# Cutaway：丢一张图，让 Agent 用 Three.js 代码建出可拆装的 3D 机械模型并拍成片（阿麦AIGC）

- **日期**：2026-11-25（收录）
- **来源**：微信公众号「阿麦AIGC」，原标题「一个极其惊艳的 Skill」
- **原文**：https://mp.weixin.qq.com/s/37eTKD2aZEEKgQd_7Z7yvw
- **项目**：https://github.com/Nyxera0x/cutaway （已核对 README：「把一张图变成会自己拆开的 3D 物体的 Claude Code skill：用 three.js 代码搭建、对照原图检查、60 fps 拍摄并配同步音效」），**MIT**
- **基于**：whaleyxbt/claude-motion（MIT）提供运行时、逐帧采集、音效引擎和审片规则；音效采样为 Freesound CC0
- **分类**：工具与产品 / Agent Skill / 代码动画 / 3D
- **相关**：[dsh-blender：AI 后台跑 Blender 建模](2026-10-07-dsh-blender-headless-ai-3d-modeling.md)（DSH 是什么见此篇）· [SenseNova Code2Video](2026-11-25-sensenova-code2video-open-source.md) · [HyperFrames](2026-10-14-hyperframes-html-to-video-render-engine.md) · [onetake](2026-10-21-onetake-one-shot-promo-video-skill.md)

## 一句话

给 Claude（或其他编程 Agent）一张机械动物、机器或设备的图，它用 Three.js 把每片外壳、齿轮、獠牙都**写成代码**重建出来，**逐条对照原图检查**，然后拍成片：光标按下按钮，那个部件在自己的镜头下拆开，最后整体爆炸再扣回去。全程纯代码驱动，没有用任何视频生成模型。

## 方法上最值得学的（README 原文要点）

- **一张参考图定观感**：Claude 读图后重建，**从不直接导入**这张图
- **为这张图写 10–12 条编号检查清单**，每一轮逐条打"是 / 部分 / 否"
- **测量而不是目测**：暗色机构所占比例、整体比例都对照原图量
- **先出静帧**：第 2 轮后停下等你确认，确认前不拍任何东西
- 拍片时**1:1 复用身形最接近的官方示例**，只换躯体；每个部件有自己的镜头
- 10 秒版有独立的脚本、开场和收尾

> 这套"参考图 → 编号清单逐条打分 → 量化比例 → 先静帧确认再开拍"的顺序，和库里 Code2Video 的"渲染后读回自检"、ffmpeg-skill 的"执行后复测"是同一种验收思路，只是落在 3D 建模上。

## 上手

```bash
npx skills add Nyxera0x/cutaway -g -a claude-code
# 然后在安装目录里，只需一次：
npm install && npx playwright install chromium
```

- 需要 Node.js 20+；导出视频需要 ffmpeg；生成音效需要 Python 3
- 先跑官方示例：
  ```bash
  node sims/build.mjs tarantula-film   # 产物 cutaway-out/dist/tarantula-film.html
  node sims/build.mjs piranha-film
  node sims/build.mjs wasp-film
  ```
- 直接用自然语言提需求，例如：
  - "这是一只机械螳螂，做成我能拆开的 3D 物体，先停在静帧让我检查。"
  - "把这张机械蟹的图做成 20 秒短片，每个部件都拆开。"
  - "静帧我确认了，按食人鱼示例的方式拍。"
- 还没有参考图：`docs/PROMPTS.md` 里有官方示例用的生图提示词

## 仓库结构

| 路径 | 作用 |
|---|---|
| `SKILL.md` | Claude 读的工作顺序、出效果的规则、审阅循环 |
| `docs/` | 完整方法，有没有代码都能照着做 |
| `sims/green/` | 每个物体外围的页面与绿色观感，附 3 个审阅工具：残留颜色探针、镜头轨迹、部件锚点 |
| `sims/kit/` | 运行时、舞台、着色器预热 |
| `sims/build.mjs` | 把物体打包成单个 HTML（three.js 和字体内联，可离线打开） |
| `sims/capture.mjs` | 逐帧渲染成 60 fps MP4，或导出静帧审阅表 |
| `sfx/`、`run_sfx.py` | 按影片时间轴生成程序化音效，母带到 −14 LUFS |

## 原文作者的扩展玩法

- 用 Three.js + SVG 复刻特斯拉 Roadster 飞行汽车夜试的长镜头（坡顶倒挂、喷雾横滚、落地避震回弹），属模拟重现
- 把拆装节奏的经验搬到 **Blender 建模**：复刻小说《蛊真人》里的"万年斗飞车"（外壳弹开，再从船尾向船首逐段扣回）和"宿命蛊"
- 在 **DSH（DeepSeek Harness）**里使用：先让 DSH 按环境装好 Skill 并跑通原版示例，再附图让它"先出静帧，确认后做 10 秒拆装动画，交付可离线打开的 HTML"，需要时加一句"同时导出 MP4"

## 点评

- 原文篇幅很短，几乎全是 GIF；真正有信息量的是 README，本笔记主要依据 README 整理
- 适用面比标题窄：它擅长的是**机械感、可拆分部件的物体**（机器人、机械动物、设备）。软体、写实生物、人物不在它的舒适区
- 产物是单文件 HTML + 60 fps MP4，适合做产品拆解演示、科普结构图解、硬核风格短片
- 和 Code2Video 一样走"代码即视频"路线：每个部件都是代码里的对象，改一处不影响别处，可复现
- 原文的 DSH 使用说明是给 DeepSeek Harness 用户的，用 Claude Code 直接按 README 装即可
