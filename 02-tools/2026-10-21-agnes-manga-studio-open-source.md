# Agnes 漫剧工坊开源：本地单文件版，MIT 协议可商用（动画大丸家）

- **日期**：2026-10-21
- **来源**：动画大丸家
- **分类**：02-tools 工具与产品（AI 漫剧 / 短剧）
- **URL**：https://mp.weixin.qq.com/s/dDFYo-9e6kaUhnbYYp68eA
- **仓库**：https://github.com/liobububu/agnes-manga-studio（MIT）
- **下载**：https://github.com/liobububu/agnes-manga-studio/releases/tag/v1.0.1

## 概述

作者原先在某国内建站平台上开发了在线版「Agnes 漫剧工坊」（AI 漫剧制作工具）。因为平台额度用完后要付费才能下载源码、还会停后端，作者花钱下载源码，用 WorkBuddy 改了几轮成**本地版**，开源到 GitHub。作者举的对照是 Grok（「马斯克那里」）：额度用完照样能直接下载 H3 导演台的网站源码，后端也不会被停。

## 关键内容

- **形态**：单文件程序包，无需安装，打开就能用。也可以把仓库地址丢给 WorkBuddy 等编码 Agent 继续改
- **用法**：和在线网页版差不多
  1. 在「设置」里填密钥。必须是 **Agnes 国际站**的 API 密钥
  2. 拉取模型列表，选要用的模型
  3. 按工坊流程生成漫剧
- **协议**：MIT，可随意修改、使用，商用也不需要额外授权
- **后续计划**：作者还打算开源「GIF 表情包工坊」和「动画大玩家 3.0」
- **其他入口**：
  - 新网页版建在 RH（RunningHub）上
  - 下不了 GitHub 的可以去作者的「漫剧宝藏资源网」下载：https://app-8cftlqqkyfpd.appmiaoda.com/resources/f6de0dbb-a033-434b-9a6f-5ce9b6196a10
- **为什么用 WorkBuddy 开发**：目前在优惠期可免费使用（文中称已接入 GPT 6，DeepSeek V4.1-Flash 免费 14 天）

## 备注

- 原文以吐槽建站平台收费为主，功能细节很少；具体的分镜、图片、视频能力需要看仓库 README
- 依赖 Agnes 国际站的 API，免费额度和政策以 Agnes 官方为准。Agnes 免费 API 系列见本库：
  - [2026-08-11 多模态 API](2026-08-11-agnes-ai-free-multimodal-api.md)
  - [2026-08-23 免费 API](2026-08-23-agnes-ai-free-api.md)
  - [2026-08-26 AI Studio v12](2026-08-26-agnes-ai-studio-v12.md)
  - [2026-08-30 WorkBuddy + Agnes](2026-08-30-workbuddy-agnes-free-api-skill.md)
  - [2026-09-14 Agnes 3 Flash](2026-09-14-agnes-3-flash-agnescode-free-ai.md)
- 单文件程序来自个人作者，运行前建议先看源码或在隔离环境中打开
