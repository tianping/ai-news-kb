# stickman-video-director：Codex Skill 跑通火柴人动画，800万播放 / 18W粉

> 来源：老王真心谈（微信公众号）
> 原文：https://mp.weixin.qq.com/s/jNKWJbxfZFCzrIp28wKLoQ
> 开源项目：https://github.com/kaomei/stickman-video-director
> 收藏时间：2026-09-13

## 背景数据

YouTube/TikTok 上跑量的火柴人动画：某创作者 34 条视频、18 万粉、单条 800 万播放。
画风成本低、辨识度高——黑白线条 + 高饱和强调色就够。

**痛点**：Gemini Omni Flash 能生成视频，但一条提示词跑出来不可控。逐条手写动画参数、镜头运动、配音节奏最耗时间。

## Skill 设计逻辑

`stickman-video-director` 把「逐条手写提示词」压成「丢文案进去」：

- 一分钟视频拆成 **6 段 × 10 秒**独立生成（Gemini Omni Flash 可控时长约 10 秒，更长画面易飘）
- Skill 当**导演**不当渲染器：
  1. 输入文案（中/英文均可）
  2. 出导演提案：英文旁白 + 六幕分镜 + 每幕镜头运动 + BGM/音效
  3. 用户确认或改两笔
  4. 输出 6 条 Gemini Omni Flash 提示词（每条含时间节拍 + 负面约束）
  5. 逐条渲染 10s 片段
  6. 拼接成一分钟成片

## 画幅与主题

| 画幅 | 适用 |
|------|------|
| 9:16 竖屏 | TikTok / Reels |
| 16:9 横屏 | YouTube |
| 1:1 方形 | 通用 |

主题：白底黑火柴人 / 黑底白火柴人，配高饱和强调色。

## 安装与前置条件

- clone `https://github.com/kaomei/stickman-video-director`，装进 Codex skills 目录
- 运行前提：有 Codex 环境
- **无需**单独 API key、**无需** MCP
- 从装 Skill 到出 6 条提示词约 10 分钟

## 音频衔接注意

6 段独立生成，旁白/BGM/音效在段间衔接要特别处理。如需帧级音频对齐、多语言配音、一键导出成品视频，可在 Skill 基础上加强。

## 适用场景

短视频创作者（YouTube / TikTok），核心诉求：把「写提示词」这个最耗时环节压缩成一步，特殊需求后期手动修。

## 要点速记

- 文案 → 导演提案 → 确认 → 6 条 Gemini Omni Flash 提示词 → 逐条渲染 → 拼接
- 10s 是 Gemini Omni Flash 稳定出片区间
- 每条提示词带时间节拍 + 负面约束
- 黑白 + 高饱和强调色即可辨识度高
- 无 API key / MCP 依赖，clone 即用
