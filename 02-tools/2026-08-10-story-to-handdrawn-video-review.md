# story-to-handdrawn-video：实测评测与使用指南

> 收藏来源：微信公众号「洞见AI」/「IT开发者生活」
> 原文链接：https://mp.weixin.qq.com/s/n0LZA69LIoM1Sw8HHMLSDg
> GitHub 项目：https://github.com/gnipbao/story-to-handdrawn-video
> 收藏日期：2026-08-10
> 关联笔记：[项目详解](2026-08-09-story-to-handrawn-video.md)

---

## 一句话说清

不是又一个"把文字变视频"的通用工具，而是一套专门把中文故事做成**手绘风格短视频**的开源工程。

## 它解决一个很窄的问题

输入一段中文故事（或一组按顺序的手绘图片），输出 3:4 竖屏、手绘风格的**静音**动画。分句、分镜、生成插画、加转场，一条龙。

和"通用 AI 视频"最大的不同是审美被钉死了——就做手绘故事这一件事，一次性给 20 种画风切换：彩铅日记、水墨写意、儿童蜡笔、木刻社论、Zine 拼贴……

> 手绘不是一种审美，是一堆审美。讲童年的用儿童蜡笔，讲人生感悟的用水墨，讲社会议题的用粗粝木刻——画风选错，故事的情绪就歪了。

## 架构

- **底层**：Remotion 工程，管分镜、动效和渲染
- **上层**：Skill，塞进 Codex、Claude Code 或 Kimi Code 等 Agent，用中文说话就能驱动

跑起来的链路：故事文本 → 自动按句分镜 → 调 Codex Image2（默认）或 OpenAI API 出彩色插画 → Remotion 在本地对齐生成黑白层 → 渲染时让"文字 → 黑白画稿 → 彩色插画"从左到右揭示，还能加右下角卷页翻书效果。

正式 1080×1440，预览 720×960，H.264 静音。20 套画风配方来自 hand-drawn-styles（MIT 署名）。

## 作者

gnipbao，独立开发者，一个人。没有融资，没有公司。目前 v1.1.0，MIT 协议。

## 本地实测记录

- **环境**：Windows + Node 22 + Python 3.13
- **clone** 几秒，`npm ci` 一分二十秒装了 186 个包
- **踩坑**：`npm run styles` 报错退出——NODE_OPTIONS 带了旧版 node 的 `--use-system-ca` 参数冲突，unset 掉就好
- **通过**：tsc 类型检查零报错；`npm run styles` 正常打印全部 20 种画风，默认是 colored-pencil-diary
- **注意**：`npm run check` 里的分镜校验会逐条核对图片是否真生成，示例分镜因没出图全报红，新手容易被吓一跳
- **未跑通**：出图依赖 Codex Image2 或 OpenAI 的 key，手头没有，没生成最终 mp4

## 分人群使用建议

- **不会写代码的**：用支持 Skill 的 Agent（Codex / Claude Code / Kimi Code），把 skill-package 丢进 skills 目录，用中文说"用这个 skill 把这段故事生成手绘动画"即可
- **会写代码的**：`git clone` → `npm ci` → `python3 scripts/run_story_video.py --list-styles` 看 20 种风格 → `--input story.txt --style ink-wash --mode plan` 出分镜
- **做自媒体的**：拿静音画面轨丢进剪映或 PR 自己配旁白和 BGM

## 边界与代价

- 只出"静音画面轨"，配音配乐需后期——设计取舍，不是偷懒
- 出图需要 API 额度
- MIT 协议，但画风配方来自 hand-drawn-styles，商用需保留 MIT 署名
- 想要"输入即出带配音成品"的，它给不了——是工具链的一环，不是全自动工厂
- 想要照片级写实或几分钟以上长片，也别指望它
