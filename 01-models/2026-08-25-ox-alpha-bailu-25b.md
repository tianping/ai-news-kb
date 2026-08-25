# Ox Alpha 身世大反转：全网猜智谱小米，它可能是武汉白鹿的 25B 模型

> 来源：[微信公众号](https://mp.weixin.qq.com/s/qPsBbiL_UgOhefgnC_14LQ)
> 收录日期：2026-08-25
> 分类：模型发布
> 声明：本文为基于公开信息的推测性分析。截至发稿没有任何一方（OpenRouter、白鹿、智谱、微软）正式认领 Ox Alpha，所有"身世归属"均为社区推断。

## 0x00 一个匿名模型，炸翻开发者圈

8 月 20 日 OpenRouter 挂出无名字、无参数、无技术报告的新模型 `stealth/ox-alpha`，价格栏两个 0（免费）。硬指标：

- 约 **1M token 上下文**（1,048,576），单次最长输出 131,072
- 文本 + 图像 + 视频多模态输入
- 强制推理（reasoning 不可关闭），max/high/low 三档强度
- 原生工具调用、function calling、结构化 JSON 输出
- 定位：长程软件工程、agentic 任务、生产级负载

Stripe CEO Patrick Collison 试后评价 "very impressive"。上线 5 天冲上 OpenRouter 周调用量榜第 4（6.54T tokens，截至 8/22 当周），排 DeepSeek V4 Flash、小米 MiMo-V2.5、腾讯 Hy3 之后。"ox"= 公牛，网友戏称"牛来"模型。

## 0x01 身世之谜：三大猜测

**智谱（Z.ai / GLM-5.3）——主流猜测一，证据最硬：**
- 视频编码管线、tokenizer 行为与 GLM 家族高度相似；API 错误格式、视频编码器与 GLM-5V-Turbo 逐 token 对齐
- 有研究者把 serving profile 与 OpenRouter 全库 422 个模型比对，10 个字段里与 GLM-5.3 对上 9 个
- 智谱有前科：Pony Alpha = 智谱 GLM-5。社区一度把置信度拉到 99%

**小米 MiMo —— 主流猜测二：**
- 上一头匿名牛 Hunter Alpha 已被证实是小米 MiMo-V2-Pro
- MiMo-V2.5 现居周调用榜第 2（9.14T、环比 +138%），coding/agentic 路线头部玩家，定位高度重合

**微软 MAI —— 少数派：**
- tokenizer 挖出 OpenAI 系 BPE 方案痕迹，反而排除智谱/Google/Anthropic/xAI，指向微软小型自研谱系（甚至 IBM 开源家族小概率）

## 0x02 新爆料：武汉白鹿的 25B？

新线索指向国产厂商**白鹿（BAILU AI）**（武汉鹿明智能科技有限公司）：

- 白鹿技术博客写过："25B 前沿，DeepSWE 超越 Kimi K3(max)、DeepSeek V4 Pro/Flash(max)"、"第四次大规模预训练达 960B、1M 上下文；首个原生多模态 MoE"——几乎是 Ox Alpha 的"招股说明书"
- 参数量级对得上：匿名模型大多靠大参数"虚胖"，白鹿走"25B 黄金参数、极致性价比"，恰好解释 Ox Alpha 不爆显存还能跑出惊艳编码成绩
- 免费 + 强编码 = 低成本收割开发者口碑的最佳姿势

## 0x03 反方证据与疑点

- **tokenizer 指纹冲突**：serving profile 与 GLM-5.3 的 9/10 匹配 + 视频编码器逐 token 对齐，是指向智谱的最硬证据。若指纹鉴定没翻车，"白鹿论"直接出局
- **就算真是白鹿，会不会是拿智谱底座调的？** 白鹿主打"原生训练、不蒸馏"；若确为白鹿出品却与智谱 serving 特征几乎重合，要么鉴定误判，要么"不蒸馏"叙事站不住
- **参数未公开**：25B 是最强推测之一，非定论
- **OpenAI 系 BPE 残留**与"纯国产"叙事相悖
- **命名口径不一**：白鹿 APEX 2.5 编码系列文档标注 262K 上下文，技术博客却写"25B 前沿，原生 1M"。本文所指对齐博客那条 1M 线

## 0x04 匿名发布成为中国模型的"标准动作"

Pony / Hunter / Elephant / Owl / Ox——中国厂商越来越喜欢"先在 OpenRouter 匿名放免费王炸，再正式官宣"：

1. 剥离品牌偏见，开发者纯看输出打分
2. 正式发布前获得海量真实反馈与口碑发酵
3. 低成本制造全球话题

## 结论

三条线都停在"疑似"，无人认领。免费窗口传闻约 **8/27 结束**，趁免费去 OpenRouter 拉 `stealth/ox-alpha` 用自己的代码仓库验真。下一头匿名牛可能已在路上——盯住 stealth/ 系列。

> "等官宣那天，所有匿名的牛，都会被认领。"

---

### 相关笔记

- [Token 自由时代的到来：Qwen3.8-27B 发布与本地模型热潮](01-models/2026-08-21-qwen38-open-source-business-guide.md)
- [DeepSeek 涨价后免费模型 API 盘点](01-models/2026-08-20-deepseek-free-api-alternatives.md)
