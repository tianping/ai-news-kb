# hand-drawn-explainer-video：「边讲边画」手绘讲解视频 Skill 开源（Nikola数说未来）

- **日期**：2026-10-15
- **来源**：微信公众号「Nikola数说未来」（作者老刘）
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/vuG9cLI_IZmpaU0BB4Q_VQ
- **仓库**：https://github.com/hi-nikola/hand-drawn-explainer-video-nikola
- **相关**：[huashu-art-motion](2026-10-14-huashu-art-motion-code-animation-skill.md)、[HyperFrames](2026-10-14-hyperframes-html-to-video-render-engine.md)

## 概述

作者（自称不会写代码）用 Codex 做了一个 Skill：丢一段稿子、一个主题或一句话，生成「一支笔在纸上边画边讲」的手绘讲解视频，直接交付完整 MP4 + 字幕 + 时间轴 + 可编辑工程文件。发到 X 上两天浏览量约 50 万，GitHub 约 375 Star（截至发文）。

## 关键内容

### 两种风格
1. **逐笔故事动画**：讲人物/历史/故事，一张纸从头画到尾，先勾线再上色（例：「乔布斯一生」「约法三章」七幕连画）
2. **HTML/SVG 程序动画**：讲方法/流程/概念，卡片、箭头、关键词依次出现，不会写错字（例：「什么是 Skill」）

画风可换：Q 版人物、白底黑线等。

### 细节设计
- 模仿真实笔触逐笔落线，不拿静图平移糊弄
- 交付前自检：有无声音、字幕是否正确、每一幕是否画完
- 配音默认 **火山引擎 TTS**（新账号送免费额度）
- 核心渲染不吃显卡、不需下载大模型

### 时间与成本
- 普通老办公本：1 分钟视频约 **20 分钟**生成，基本一次成
- 成本 = Codex token + TTS 费用（几毛钱）；ChatGPT Plus 一天约能跑两三条
- 作者建议用 GPT/Codex；豆包、WorkBuddy 理论可行但效果差

### 一句话开工提示词
> 安装 https://github.com/hi-nikola/hand-drawn-explainer-video-nikola，用这个 Skill，把下面内容做成 45 秒中文手绘讲解视频，讲一部分画一部分，先画左边再画右边，最后给我 MP4、字幕和可编辑素材。

## 适用人群

知识视频号、老师、培训科普——需要把复杂事讲明白的人。

## 备注

- Star 数、浏览量为作者自述；未实际运行验证。
- 与 huashu-art-motion（代码画动画）、story-to-handdrawn-video（本机 OpenClaw 已有的手绘故事视频 skill）思路相近，可对比选用。
