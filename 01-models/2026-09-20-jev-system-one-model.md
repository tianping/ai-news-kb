# Jev 突然爆火：不会聊天不写代码，只做判断的 System One Model

## 元数据
- 日期：2026-09-20
- 来源：华尔街见闻
- 分类：01-models（AI 模型）
- URL：https://mp.weixin.qq.com/s/qcONLK_UIpIKXdNoXOO18w

## 概述
Jev 是一款反常的 AI 模型：不会聊天、不写代码、不生成大段答案，只做三件事——Yes/No 判断、选项选择、打分。正是这种"能力被砍了一大半"的设计，让它在开发者圈突然爆火。TypeSafe 测试显示最高提速 193.6 倍、成本降低 444.6 倍，输入每百万 Token 仅 $0.042，输出免费。端到端延迟 70-500 毫秒。

## 核心设计：System One Model
概念来自丹尼尔·卡尼曼《思考，快与慢》中的系统 1（快速直觉）vs 系统 2（慢速深思）。Jev 承担 Agent 运行中海量的小判断：
- 下一步调用哪个工具
- 该点网页哪个按钮
- 这条信息还有没有用
- 任务到底完成没有
- 结果要不要重新检查

一个 Agent 一天可能需要几十万到几百万次判断。每次调用最强模型做"深思熟虑"成本太高，Jev 抢的就是这一层。

## 创始人背景：RLHF 的反思者
Diogo Almeida，曾参与 OpenAI GPT-4、ChatGPT、InstructGPT/RLHF 相关工作，是 OpenAI 内部少数公开"黑"ChatGPT 的人。一个多月前演讲《What's Next After RLHF？》，被视为 Jev 的"思想说明书"。

核心论点：Today's AI is incredible at assistance, not automation.
- 协助时代：Claude Code 再强，你仍坐在屏幕前看它写、检查它改
- 真正自动化：人不在场，AI 后台自己判断执行，一天几十万上百万次

## 为什么 RLHF 不适合自动化
RLHF 训练时把"人塞进回路"，模型学习"什么样的回答人更喜欢"。这导致：
- Overpromising is a feature（过度承诺是特性不是 Bug）
- 即使不知道也说得像那么回事（如 ChatGPT 夸放屁录音为环境音乐）
- 聊天产品用户在场可纠正，但自动化系统需要的是：怎么做 + 多大把握

## Jev 的训练方法：RLCD
Reinforcement Learning for Calibrated Decisions（校准决策强化学习）。解决的核心问题：如果 AI 说 80% 概率，这 80% 能不能信？
- 理想情况：模型判断 80% 概率的一批事，最后约 80% 真的发生
- 99% 把握 → 直接执行
- 51% 把握 → 转给更强模型或人
- 真正麻烦的不是 AI 不知道，是 AI 不知道自己不知道

## Jev 的三个功能
1. **Noul**：Yes/No 回答
2. **Choice**：从选项中选一个
3. **Score**：按标准打分
全部直接返回判断 + 概率，不生成文本。

## 实际用例
- 40 秒分析 724 条实时广告，做出 8724 次判断
- 接入 Claude Code 清理无用上下文
- 作为 AI Agent 裁判检查任务完成度
- LangChain 已开始测试 Jev 作为 Agent 评估器

## 局限与备注
- "零幻觉"仅指不跳出规定答案类型乱编，不代表不会选错
- 极端性能数据（193.6x、444.6x）来自 TypeSafe 自家测试
- 尚不能断言代表下一代 AI，但提出的底层问题值得跟进

## 参考链接
- 微信原文：https://mp.weixin.qq.com/s/qcONLK_UIpIKXdNoXOO18w
- TypeSafe（Jev 开发方）