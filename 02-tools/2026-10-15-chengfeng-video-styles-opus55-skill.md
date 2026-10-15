# chengfeng-video-styles：Opus 5.5 一段话到成片，16 种风格 Skill 免费开源（AI产品自由）

- **日期**：2026-10-15
- **来源**：微信公众号「AI产品自由」
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/1e3Y0n7R7vsjVTO-Fs3e7Q
- **仓库**：github:Agentchengfeng/chengfeng-video-styles
- **相关**：[Opus 5.5 视频万能提示词 + GitHub 项目](2026-10-08-opus55-video-universal-prompt-github-projects.md)、[hand-drawn-explainer-video](2026-10-15-hand-drawn-explainer-video-skill-nikola.md)

## 概述

作者把自己用 Opus 5.5 做视频的整套做法打包成 Skill「chengfeng-video-styles」，内置 16 种来自其实际作品的风格，也能逐帧分析参考视频/图片复刻新风格。核心流程：**先出 3 秒样片挑风格，再出全片**（避免整片跑完额度耗尽却不满意）→ HTML 成片 → 导出 MP4 → 代码合成音效 + 字幕。其他模型可用但效果较差。

## 七步流程

1. **安装**：对 Agent 说
   > 帮我安装视频风格 Skill：chengfeng-video-styles
   > 安装命令：`npx -y github:Agentchengfeng/chengfeng-video-styles install`

   装好后**新开 Agent 会话** Skill 才出现
2. **给内容**：`/chengfeng-video-styles` + 一篇文章 / 一个概念 / 一段话即可
3. **定参考（可选）**：要 16 种之外的风格，把最满意的视频或图片拖进来说「复刻」，Opus 5.5 逐帧分析每段时长、镜头运动、转场后照做（作者示例：纸艺风格）
4. **看 3 秒样片**：每种风格各做一个 3 秒样片（16 + 1 个复刻参考 = 17 个），只做内容里最出彩的一拍；汇总到网页自动打开，没弹出就在 Agent 工作目录找 `样片筛选.html`
5. **出全片**：告诉它选第几个；产出 HTML 文件，浏览器打开循环播放
6. **导出 MP4**：说「导出成 MP4」，实测约 4 分半
7. **加音效和字幕**：说「加上音效和字幕」，无需素材，音效代码合成，实测约 20 分钟，自动重导带声音字幕的 MP4。**它自己听不到声音**——人工听一遍，指出第几秒不对，让它单独调

## 要点

- 样片先行是控制额度与效果的关键
- 风格库可通过参考视频持续扩展

## 备注

- 仓库与用时为作者自述，未实际运行验证。
- 与库内多篇 Opus 5.5 代码生成视频笔记（09-26 工作流、10-08 万能提示词等）同一路线，本篇特点是「风格库 + 样片筛选页」的工程化封装。
