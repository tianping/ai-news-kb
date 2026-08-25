# Ox Alpha 又反转了：所有指纹都指向 GLM，但出现了一个解释不通的地方

> 来源：[阿涛 AI 观察（微信公众号）](https://mp.weixin.qq.com/s/G7tUcFXJrnCOmiwm6kd9FA)
> 收录日期：2026-08-25
> 分类：模型发布
> 关联：[Ox Alpha 身世大反转：全网猜智谱小米，它可能是武汉白鹿的 25B 模型](2026-08-25-ox-alpha-bailu-25b.md)

## 背景

`stealth/ox-alpha`：OpenRouter 上的匿名模型，免费、1M 上下文、能写代码、长跑 Agent、支持视觉输入。官方声明只说"来自选择匿名的第三方提供商"。开发者社区进入"模型刑侦"模式，线索越来越集中指向 GLM。

## 指纹一：Tokenizer「验出 DNA」

- 问模型身份无意义：数百次诱导（角色扮演/Base64/摩斯密码/Prompt Injection）它始终坚持自己是 ox-alpha，隐藏指令明确要求只能以此身份回答
- 多语言 + 代码 + Emoji 分词对比测试：一项测试中 Ox Alpha 与 GLM-5.3 每组 Token 数完全对应，仅固定多出 **75 个 Token**（疑似偷塞的 System Prompt）
- 更大规模独立测试（44 个区分度字符串）：**GLM-5 代 44/44 全匹配**；GLM-4.x 42/44，Qwen3 38/44，Kimi 30/44，DeepSeek-V3 28/44
- 决定性细节：👋 和 🔥 两个 Emoji 的分词差异刚好区分 GLM-4 / GLM-5 系列——Ox Alpha 全部站 GLM-5 这边

## 指纹二：报错信息一样

GLM-5.3 的 reasoning 不能完全关闭（low/high/max），传非法参数会报错。故意弄坏 Ox Alpha：返回 `[1210]` 开头错误，与 GLM-5.3 参数验证行为高度一致；另一组测试触发中文验证信息和 `[1301]` 内容审核错误——**匿名模型后面服务器突然开始说中文**。（保留：Provider 可自建代理层包装错误）

## 指纹三：图片切分方式像 GLM

输入不同尺寸随机噪声图片记录 Token 消耗：Ox Alpha 采用 **14×14 patch + 2×2 合并 + 尺寸向 28 倍数对齐**的视觉编码方式，18 种尺寸全部可预测。

## 反证一：知识截止日期对不上？

8 月 24 日有用户测出 Ox Alpha 知识在 **2025 年中后段断掉**，而 GLM-5.3 能知道 2026 年信息。但另一更系统的调查得出的是 **2025 年 10~11 月附近**：知道 GLM-4.6/4.5、Claude Sonnet 4.5、Qwen3、GPT-4.1、o3/o4-mini，不知道 GLM-4.7 及以后——反而像"诞生在 GLM-5 训练阶段附近、还不知道 GLM-5 正式发布的 checkpoint"。

且直接问模型知识截止日期，答案本身互相矛盾——自我报告 ≠ 黑盒推断的真实训练数据时间（可记错/幻觉/被 System Prompt 影响/后训练注入新知）。

## 反证二（真正难解释）：它会看图

公开版 GLM-5.3 是纯文本模型；Ox Alpha 实测能 OCR、看数学题、识图、分析网页截图、处理视频帧。所以 **"Ox Alpha = 当前公开 API 上原封不动的 GLM-5.3" 越来越难成立**。但 Z.ai 有独立视觉路线 GLM-5V-Turbo（原生 Text/Image/Video/File + 强化 Coding/Agent）。

## 三个假说（合理性从高到低）

1. **未公开的 GLM-5 系列多模态 checkpoint**：文本部分属 GLM-5 家族（Tokenizer/reasoning/API/风格全对上）+ 新 Vision Encoder（能看图看视频）。最能解释所有现象
2. **以 GLM 为底模大量二次训练**：Continued Pretraining/Fine-tuning/RL/Vision Adapter。但 GLM-5.3 权重未公开，第三方从哪获得是新的问题
3. **不是 GLM 但复用整个 GLM 技术栈**：权重不同但复用 Tokenizer/Chat Template/API 层/Vision Encoder/Serving Stack。技术上可能，但巧合程度需要越来越多额外解释——奥卡姆剃刀反而指向 GLM 家族

## 结论

- 可以说：黑盒证据强烈指向 Ox Alpha 与 Z.ai/GLM-5 技术家族有非常深的关系
- 不能说：确认就是 GLM-5.3；智谱已认领（截至发稿均未官宣）
- "具体是哪一个 GLM"成了最大悬念

## 更大的看点：大模型数字取证

社区已经从"猜模型"进化到系统性 **Model Fingerprint** 取证：Tokenizer、Emoji 分词、Hidden Prompt Offset、API Error、Reasoning 参数、Vision Patch、Context Window、Knowledge Timeline……就像浏览器有 Browser Fingerprint。以后厂商偷偷换后台模型，开发者跑几十个 Probe 就能发现，不用等公告。

> 案子还差最后一页：官方认领。在那之前最准确的结论依然是：很像 GLM，而且已经不是一般地像。
