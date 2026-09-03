# Meta Muse Spark 1.3 发布：便宜到忽略不计的"性价比之王"，小扎对谷歌贴脸开大

- **来源**: [新智元](https://mp.weixin.qq.com/s/lrLwfq6p5GCxlNtfi13lMg)
- **日期**: 2026-09-03
- **背景**: 九月前 3 天已有 5 个前沿模型发布——Anthropic Fable 5.1 / Mythos 5.1、谷歌 Gemini 3.8 Flash / Cyber、Meta Muse Spark 1.3

## 核心亮点

- **VS 1.2**：无效轮次减少，工具调用次数 ↓~20%，token 消耗 ↓~25%，长周期任务中更好保持需求
- **发布时间微妙**：Gemini 3.8 Flash 刚当了几小时"轻量霸主"就被踢馆
- **定价**（加量不加价）：输入 $1.25/M token、输出 $4.25/M
  - 贡献者版 `muse-spark-1.3-contributor` 是国外模型定价地板（代价：数据用于改进 Meta 产品）
  - Mehul Mohan 称这是美国 AI 实验室的 **"DeepSeek 定价时刻"**

## 跑分情况

| 基准 | 分数 | 对比 |
|------|------|------|
| DeepSWE v1.1（长程编程） | **75.4** | Claude Opus 5: 74.0 / GPT-5.6 Sol: 73.0 |
| SWEAtlas CodeBase QnA | 59.4 | 超 GPT-5.6 Sol 和 Opus 5 |
| Terminal-Bench 2.1 | 88.8 | |
| MRCR 512K-1M | **98.1** | GPT-5.6 Sol 仅 73.8（碾压级） |
| Artificial Analysis 智能指数 | 62（Max 推理） | 与 Claude Fable 5 持平，第一梯队 |
| Agent 能力（JobBench/OSWorld） | — | 与 Opus 5 差距几个百分点 |

## "Less is More"：1 美元跑 2 小时

- 不做"大力出奇迹"堆参数，而是**做减法**：专门训练长周期编程+指令遵循，减少交互轮次，输出更简洁
- 开发者 @SPAC89 实测：Ultra Contributor 模式 vs Fable 5.1 xHigh，附带"自评分低于 9.5/10 必须继续改进"规则
  - 2 小时内跑 **20 轮自我改进循环**，每轮 3 个智能体 ≈ 60+ 次 agent 运行
  - **总成本 < $1**
- 同等智力水平（≥59 分）单任务成本对比：
  - Muse Spark 1.3: **$0.55**
  - Grok 4.6: $0.94 / GPT-5.6 Sol: $0.95 / Claude Opus 5: $1.23

## 战略解读

- **Alexandr Wang**（Meta 超级智能实验室掌舵人）连发三条 X 造势；接受采访时明确：所有改进服务于小扎蓝图——**7×24 小时个人智能体**
- 个人智能体的两个前提：
  1. **极度聪明且稳定**：撑住长线程、并行多工作流；指令模糊会主动提问、遇阻会求助、关键操作前确认、不产生幻觉
  2. **极度便宜**：一天几十美元就永远只是少数人的玩具
- North大学（北大）校友、Meta 研究员**孙之清**透露：对 avocado 模型做了再次后训练，实现更好的 test-time scaling
- Meta 五个月迭代 4 个 Muse Spark 版本（7 月 1.1 主打 1M 上下文+计算机操作）

## 质疑声音：刷榜争议

- **BridgeMind**：本月 AI 发布如出一辙，分数飙升但真实世界体验没长进；"对基准测试的信任每周都在减少"
- 网友压力测试：Muse Spark 1.3 和 Gemini 3.8 Flash 在后端级 3D 约束下都露怯
- Artificial Analysis 深报：**AA-Omniscience 测试中 xhigh/max 版本退步**——弃权率变高（不确定时不答而非胡编）；降低了幻觉率，但说明绝对知识储备提升没跑分夸张
- 趋势判断：各家陷入"为了跑分而训练"的怪圈，DeepSWE/Tau3-Bench/GPQA 成了刷题目标

## 行业判断

1. **参数红利见顶**：单纯堆规模边际效益递减；未来胜负手是架构创新（如"循环深度"推理）+ 工具调用减法
2. **智能体工程才是胜负手**：从"谁更会说话"转向"谁更能干活"；核心壁垒是极低成本下长程任务的真实落地
3. 用不到 1 美元跑完 60 次试错循环的模型，将重塑商业模式

## 参考链接

- [Meta 官方博客](https://research.meta.ai/blog/introducing-muse-spark-1-3)
- [Artificial Analysis 模型页](https://artificialanalysis.ai/zh/models/muse-spark-1-3)
- [开发者实测 X 帖](https://x.com/SPAC89/status/2095284969614475744)
