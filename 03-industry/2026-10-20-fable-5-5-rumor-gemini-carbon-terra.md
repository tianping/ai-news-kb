# Fable 5.5 传将上线：主打「读懂意图」，谷歌 Gemini 4 Carbon、OpenAI 新 Terra 同步备战

- **日期**：2026-10-20（收藏）
- **来源**：微信公众号「新智元」（编辑：桃子、摩西）
- **原标题**：刚刚，Fable 5.5被爆明天突袭！谷歌OpenAI全面迎战
- **分类**：03-industry（行业动态 / 发布传闻）
- **URL**：https://mp.weixin.qq.com/s/eJCLy09iQFxYY7K6xLmFMA
- **一手来源**：The Information《Anthropic readies model that reads users' intentions better》；X @steph_palazzolo、@kimmonismus

> ⚠️ 全文是**传闻与爆料汇总**，收藏时 Anthropic、谷歌、OpenAI 都未官宣。「明天上线」是网友猜测，报道记者 Stephanie Palazzolo 已改口为「未来几周」。

## 概述

据 The Information 报道，Anthropic 的新模型已经交到测试者手里。早期用户说，它的提升集中在**推理深度、直觉判断、读懂人类意图**三方面，推理和直觉都超过 Anthropic 目前在售的所有模型。报道没有点名它属于 Fable（最大一档）还是 Opus（第二档）家族，但网上普遍押注是 **Fable 5.5**。

## Anthropic：新模型的传闻能力

- **读懂没说出口的目标**：用户给的信息很少、目标也没明说时，它也能看出对方真正想要什么。
- **修一个 Bug，揪出一整类**：顺着报错找到同类问题，一并列出来，再问用户要不要一起修，不会自作主张扩大修改范围。
- **跳出常规套路**：问它怎么炒股，它不讲市盈率，而是自己设计算法预测走势。TestingCatalog 评论：如果真能稳定做到跳出框框思考，AI 就算迈进了「创新者」阶段。

**旁证：现役 Claude Code 的「多想一步」**（Reddit 帖子，转到 X 后单条约 22 万浏览）
- 用户只是让 Claude Code 写个 Mac 温度监控小程序。Claude 发现 CPU 峰值 112°C，就去查谁在占 CPU，揪出挖矿程序 **xmrig**（CPU 约 1175%，12 核全满）。
- 程序藏在 `/tmp/rigupdater`，开机自启项 8 月 29 日就已埋下；Claude 先问「现在可以清理了吗」，没有擅自动手。

## Fable 5.5 的风声时间线

- **10 月初**：开发者反映 Fable 5.1 被悄悄路由到一个更强的模型上，证据是它能答出新梗「重置哥」Tibo 是谁。
  - 有人只写了 4 行提示词，让它用 JS 逐帧画动画《一个人的一生》，连跑 2 小时、用掉 2,700 万 token。
  - 和 GPT-6.1 Sol 比做可拖拽的 3D 水母，拉伸、回弹效果更好。
- **10-13**：GitHub 机器人在 Microsoft Foundry 名下登记了 `claude-fable-5-5`；LiteLLM 同日加入 `azure_ai/claude-fable-5-5`（说明称名字来自 Azure 模型接口），之后条目被删。
- **10-19 前后**：The Information 执行主编 Amir Efrati 在 X 上发问「Anthropic 是不是快发 Fable 5.5 了？看起来是……」。
- 网友猜「明天」，是因为 Fable 5.1 和 Opus 5.5 都在周二发布。

## 谷歌：Gemini 4 Carbon

- Gemini 4 Argon 上周刚发，下一代 **Carbon** 已在内部编程平台 Jetski 上跑（Business Insider 看到了内部文档和截图）。
- 员工评价：写代码「感觉像 Opus 5.5」，「Carbon 真的很好」；The Information 也称 Carbon 比 Argon 更强。
- 视觉仍是谷歌强项：开发者说看视频、测网页能不能正常跑，Gemini 依然领先。
- **定价对标 Opus 5.5**：Argon 首发优惠期只要 Opus 5.5 的一半，11-30 优惠结束后与 Opus 5.5 同价。

## OpenAI：新一代 Terra 与下一代旗舰

- 中档 **Terra**（定位在 Sol 和 Luna 之间）可能在未来几周推出：能力更强，价格比 GPT-6.1 Sol 还低（Sol 上月底才发，表现接近旗舰 Astra）。
- 旁证：微软一个 PR 把 18 份评测配置里的 GPT-5.6 Terra 统一换成了 GPT-6.1 Terra。
- 下一代旗舰用了部分**循环 Transformer**结构，全公司都在做安全保障，上线还要等一阵。

## 作者判断

- 长任务（上百万 token、整个代码库）上，「聪明」最值钱：文中说领先 10 分的模型流量能多出一倍以上，编程领域多 72%（未给口径）。
- 三家各押一头：谷歌补视觉，OpenAI 打价格，Anthropic 押「读心」，也就是在长任务里判断下一步该往哪走。

## 备注

- 发布时间、所属家族、能力描述都来自匿名测试者和媒体报道，**未经官方确认**；正式发布后应补一篇官方笔记，并回头更新本条。
- 相关笔记：[Claude 新订阅 KYC 与 Haiku 5.5 补发](2026-10-08-claude-new-subscription-kyc.md)（当时只差 Fable 5.5）、[Anthropic 蒸馏防御与封号](2026-10-14-anthropic-distillation-defenses-account-bans.md)。
