# Free Claude Code：4.8 万 Star，把 50 多个平台的免费 AI 额度收进一个界面

> 来源：[微信公众号](https://mp.weixin.qq.com/s/VREwwfU2opxzyoIWteXPkA)
> 收录日期：2026-08-25
> 分类：工具与产品
> 开源地址：https://github.com/Alishahryar1/free-claude-code

## 痛点

免费 tier 散落在五十多个平台（NVIDIA、Groq、OpenRouter、硅基流动、DeepSeek……），挨个配置要改 endpoint、配 token，一天就过去了。前 Free Claude Code 项目半年冲到 **4.8 万 Star**。

一行脚本装好 → 本地起代理 + 管理界面，50+ provider 免费额度集中管理，可搜索可排序。装完在 Claude Code 里敲 `/model`，FCC 模型直接出现在原生选择器里，全程无感。

## 原理：接口适配四要点（无黑魔法）

1. **本地代理冒充 Anthropic 接口**——Claude Code 连的是本地兼容代理，后面接哪家模型 FCC 说了算
2. **一份模型目录喂给 9 个 agent**——Claude Code、Codex、Pi、OpenCode、Cline、Hermes、DeepSeek Harness、Grok Build、Muse Code 共用同一份清单，换客户端不用重配
3. **自动故障切换**——重试耗尽后自动切下一个排好的模型，对话不用重开
4. **五个优化省九成输出 token**——可选 RTK 过滤常见命令输出 + 速率探测、命令前缀识别、标题、建议、路径等本地优化

## 上手

```bash
# macOS / Linux
curl -fsSL "https://raw.githubusercontent.com/Alishahryar1/free-claude-code/main/scripts/install.sh" | sh
```

安装时选要接的 coding agent；`fcc-claude` 进入接好免费模型的 Claude Code，另有 `fcc-codex`、`fcc-opencode`。语音：本地 Whisper 或 NVIDIA NIM 转文字即可对着 agent 写代码；手机端走 Telegram/Discord 发指令。

## 友情提示

- 第三方开源项目，README 明确不被 Anthropic 背书
- 免费额度由各家 provider 决定，随时可能收紧取消，不是永久饭票
- 接口走各家公开免费 tier、ToS 友好，本地代理只做接口适配与聚合，不碰破解；合规自己掂量
- 作者自用姿势：主力订阅 + FCC 顶轻量活省着花

---

### 相关笔记

- [Codex Router：在 Codex 里用 Anthropic/Kimi/DeepSeek/Grok 等外部模型](2026-08-23-codex-router-external-models.md)
- [DeepSeek 涨价后免费模型 API 盘点](../01-models/2026-08-20-deepseek-free-api-alternatives.md)
- [Google 面向高校学生免费送一年 AI 订阅](2026-08-20-google-ai-student-free-year.md)
