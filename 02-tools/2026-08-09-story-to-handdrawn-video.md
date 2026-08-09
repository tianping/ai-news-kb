# story-to-handdrawn-video：中文故事生成手绘动画

> 收藏来源：微信公众号「CodeAI研习社」
> 原文链接：https://mp.weixin.qq.com/s/sRx8deSjy5FaEx2PfKoTvg
> GitHub 项目：https://github.com/gnipbao/story-to-handdrawn-video
> 收藏日期：2026-08-09

---

## 项目简介

**story-to-handdrawn-video** 是一个开源项目，基于 [Remotion](https://www.remotion.dev/) 构建，可将**中文故事文本**或**一组有序手绘图片**自动转化为 **3:4 竖屏手绘故事动画**。输出为无配音、无音乐的 H.264 静音画面轨，方便后期配音。

项目包含两部分：
1. **渲染器项目（根目录）**：Remotion 工程，负责分镜、动效和渲染
2. **Codex / Agent Skill（skill-package/）**：可分发的 Skill，装入 Codex 等 Agent 后用自然语言驱动渲染器，无需手动跑脚本

## 核心功能

- 中文故事自动分句和动态分镜，保留原文措辞
- 上传漫画页或完整图片，保持原顺序和构图
- 自动拆分上方文字区与下方插画区
- 本地生成与彩色插画对齐的黑白层
- **文字 → 黑白画稿 → 彩色插画** 从左到右揭示动效
- 可选右下角卷页翻书转场（纸背保留淡化的原页纹理）
- 1080×1440 正式渲染和 720×960 快速预览
- 支持 Codex Image2 工作流，以及 OpenAI API 工作流
- **20 种内置手绘风格**，支持编号、英文 id、中文名和别名选择
- 每种风格附带固定示例图，并提供统一场景的风格总览

## 环境要求

- Node.js 20+
- Python 3.10+
- FFmpeg（ffmpeg、ffprobe 可从终端调用）
- npm
- Google Chrome 或 Remotion 管理的兼容浏览器
- 支持 Skill 的 Agent 运行时（Codex、Claude Code、Kimi Code 等）

## 快速上手

### 1. 安装渲染器项目

```bash
git clone https://github.com/gnipbao/story-to-handdrawn-video.git
cd story-to-handdrawn-video
npm ci
npm run check  # TypeScript 检查 + 分镜结构校验
```

### 2. 安装 Skill 到 Agent

```bash
# Codex
cp -R skill-package/story-to-handdrawn-video ~/.codex/skills/

# Claude Code / 通用 Agent
cp -R skill-package/story-to-handdrawn-video ~/.claude/skills/

# Kimi Code
cp -R skill-package/story-to-handdrawn-video ~/.agents/skills/
```

### 3. 设置渲染器路径

```bash
export STORY_VIDEO_PROJECT=/absolute/path/to/story-to-handdrawn-video
```

### 4. 使用示例

**故事文本 → 手绘动画：**
```
使用 $story-to-handdrawn-video 把这段故事生成可后期配音的手绘动画。
<在这里粘贴故事文本>
```

**上传图片 → 手绘动画：**
```
使用 $story-to-handdrawn-video 把这几张图片按顺序生成手绘动画:
/absolute/01.jpg /absolute/02.jpg /absolute/03.jpg
```

**翻书效果：**
```
使用 $story-to-handdrawn-video 把这些图片做成翻书效果的手绘动画:
/absolute/01.jpg /absolute/02.jpg
```

**先出预览版（720×960）：**
```
使用 $story-to-handdrawn-video 先给这个故事生成一个预览版。
```

## 20 种内置手绘风格

| # | Style ID | 中文名 | 视觉特征 | 推荐题材 |
|---|----------|--------|----------|----------|
| 1 | colored-pencil-diary | 彩铅日记漫画（默认） | 笨拙黑色毡尖笔轮廓、低饱和彩铅乱涂、大留白 | 家庭、生活、纪实情感 |
| 2 | minimal-line-explainer | 极简黑白线条讲解 | 米白纸、细黑单线、火柴人与极少道具 | 科普、流程、观点 |
| 3 | kid-crayon | 五岁儿童蜡笔坏画 | 歪扭比例、线条不闭合、明亮蜡笔涂出边界 | 童年、亲子、轻喜剧 |
| 4 | rawkid-crayon | 潦草家庭投稿蜡笔 | 家长歪线稿、孩子粗乱上色、大片露白 | 家庭连载、温暖日常 |
| 5 | bean-doodle-infographic | 小豆人涂鸦信息图 | 黑色圆豆人、白点眼、单一橙色强调 | 步骤、清单、知识卡 |
| 6 | ms-paint-bad-doodle | 鼠标烂涂鸦 | 锯齿鼠标线、荒谬比例、粗糙纯色块 | 吐槽、反转、荒诞 |
| 7 | ballpoint-scribble | 圆珠笔缠绕线速写 | 单色圆珠笔缠绕线、疏密塑形、现场手稿感 | 肖像、动物、独白 |
| 8 | real-crayon-paper | 真实蜡笔纸实拍 | 可见纸纹、蜡质结块、压力变化与大量漏白 | 儿童视角、成长记录 |
| 9 | ink-wash | 水墨写意 | 宣纸、浓淡干湿、飞白枯笔与朱红点睛 | 文化、寓言、感悟 |
| 10 | emotional-watercolor-sketch | 情绪叙事淡彩速写 | 靛蓝松散速写、透明淡彩、单一暖橙焦点 | 回忆、关系、克制纪实 |
| 11 | retro-gouache-concept | 中古动画水粉概念稿 | 奶油纸、水粉大形、橙蓝互补、干刷边缘 | 怀旧、城市、温暖剧情 |
| 12 | sunlit-storybook | 暖光童画绘本 | 柔软水粉、暖边光、蓬松形状与未完成感 | 治愈、童话、亲情 |
| 13 | nordic-gouache-storybook | 北欧低饱和水粉绘本 | 丹宁蓝与芥末黄、哑光颗粒、安静留白 | 日常、自然、睡前故事 |
| 14 | inked-storybook | 墨线淡彩绘本 | 清晰墨线、轻薄水彩、角色表演突出 | 角色、青春、对白 |
| 15 | warm-flat-storybook | 暖色几何扁平绘本 | 简化几何块面、暖色平涂、清楚视觉层级 | 关系、品牌、轻科普 |
| 16 | naive-marker-notes | 稚拙马克笔笔记 | 粗黑马克笔、荧光重点与随手批注感 | 社媒、观点、年轻化内容 |
| 17 | zine-riso-collage | Zine 孔版拼贴 | 复印颗粒、撕纸拼贴、有限孔版套色 | 成长、旅行、音乐文化 |
| 18 | organic-contour-doodle | 有机轮廓品牌涂鸦 | 松弛轮廓、温暖点色、生活方式插画感 | 餐饮、生活方式、品牌故事 |
| 19 | whiteboard-explainer | 白板讲解动画 | 白底黑线、少量红蓝标记、步骤清晰 | 教程、商业解释、时间线 |
| 20 | linocut-editorial | 粗粝木刻社论插画 | 高反差刻痕、套色偏移、纸张颗粒 | 社会议题、历史、寓言 |

## 输出规格

| 输入 | 模式 | 输出路径 |
|------|------|----------|
| 故事文本 | 正式 | out/picture_silent.mp4 |
| 故事文本 | 预览 | out/picture_silent-preview.mp4 |
| 上传图片 | 正式 | out/uploaded_picture_silent.mp4 |
| 上传图片 | 预览 | out/uploaded_picture_silent-preview.mp4 |

- 分辨率：正式 1080×1440，预览 720×960
- 编码：H.264，静音

## 使用建议

- 故事文本默认一个完整句子一个节拍；想控制节奏，直接在故事里按句分行
- 遇到时间跳跃、指代不明、医疗场景或年龄敏感角色时，建议先让 Agent 给出视觉规划，确认后再生成
- 默认使用 Codex Image2 生成图片；只有明确要求时才会走 OpenAI API（需 OPENAI_API_KEY）
- 输出是静音画面轨，配音和 BGM 属于后期工作

## 项目结构

```
├── src/               # Remotion 组件（场景、擦除动效、翻页、缓动）
├── scripts/           # 渲染器入口与导入/校验/打包脚本（由 Skill 调用）
├── skill-package/     # 可分发的 Codex / Agent Skill
├── examples/          # 示例故事文本
├── references/        # 20 风格配方、默认风格参考板与示例图库
├── public/            # 字体与素材（generated/ 为运行时产物）
├── storyboard.json     # 默认文本故事分镜示例
├── storyboard.uploaded.json  # 上传图片分镜示例
└── DESIGN.md          # 设计说明
```

## 许可证

- 项目：MIT
- 字体：站酷马善政毛笔字体（Ma Shan Zheng），SIL Open Font License
