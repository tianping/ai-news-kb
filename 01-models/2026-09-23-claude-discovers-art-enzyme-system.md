# Claude 自主发现噬菌体全新酶系统 ART：2.1 亿 Token、950 Agent、21 小时

> 来源：微信公众号「新智元」，作者：桃子
> 原文链接：https://mp.weixin.qq.com/s/OCpa097QxItWl7fVX7tOaQ
> 收录时间：2026-09-24
> 类型：AI × 生命科学 · 科研范式变革

## 核心事件

**2026 年 9 月 23 日**，Anthropic 宣布其 Claude 模型在噬菌体（感染细菌的病毒）DNA 中，自主发现了一套人类此前从未记录过的全新酶系统——**ART（Array-Associated Reverse Transcriptase，阵列关联逆转录酶系统）**。

发现过程：约 **950 个 Claude Agent** 并联运行 **21 小时**，吞吐约 **2.1 亿 Token** 的生物基因组数据，扫描超 **20 万个逆转录酶（RT）**，筛选出 3500 个候选系统，最终提炼出 20 个最具潜力对象，其中一个 Agent 在扫描巨型噬菌体（Jumbo phage）原始 DNA 时发现了反常的串联重复阵列模式。

随后，Anthropic 自己的分子生物学实验室进行了湿实验验证，确认这是一套全新的酶系统。

---

## ART 是什么？

ART 主要存在于**噬菌体**中，由三部分构成：

1. **逆转录酶（RT）** — 将 RNA 逆转录为 DNA 的酶。该 RT 本身在之前的研究中已被识别过（来自巨型噬菌体），但被研究人员当作普通噪音跳过了。
2. **伴侣基因** — 紧邻 RT 的未知功能蛋白基因
3. **DNA 重复阵列** — 一长串间隔均匀、排列规则的 DNA 重复序列，与 CRISPR 阵列结构高度相似

### 关键突破点

- **逆转录酶本身不新**，但 ART 的**定义特征**（非编码 DNA 重复阵列 + 额外伴侣蛋白）是 Claude 首次注意到的人类未知组合
- CRISPR-Cas 系统中，阵列可产生引导 RNA，帮助蛋白识别目标——这就是可编程基因编辑的基础
- ART 阵列的初步实验显示也会表达出一组不同的短 RNA，暗示可能存在类似的工作逻辑，但**具体功能尚未确定**
- ART 系统的重复阵列有 **3–21 个 DNA 重复**

> Claude Agent 的原始发现日志："The DNA next to the RT is spectacular: I can see by eye a tandem repeat array … that's a CRISPR-like … repeat array?!"

---

## 发现过程：950 个 Claude Agent 如何协作

### Anthropic 分子生物学实验室工作流程

1. **文献阅读与方法验证**：Claude 先读文献，用公开数据复现已有结果，检查方法是否靠谱
2. **候选搜索**：搜索无法归入已知系统的候选，写出功能猜想和支撑证据
3. **自我审查**：回头审视自己的判断，大量候选在此阶段被淘汰
4. **人类审查**：通过审查的候选进入实验室
5. **湿实验验证**：由人类科学家完成所有实验，Claude 协助解释结果

### 本次发现的具体路径

- 收集超过 20 万个逆转录酶序列
- 筛选出 3500 个全新候选系统
- 提炼出最核心的 20 个进行深度分析
- 第 21 小时：一个 Claude Agent 扫描巨型噬菌体原始 DNA 时发现反常信号
- Claude 进行自主分析：测算序列特征、比对微观拓扑、全网文献检索相似先例
- 生成详尽实验验证报告，提交人类科学家审查

### 人工参与度

Anthropic 团队仅做了两件事：**给出最初的探索 Prompt** + **开展实验验证**。AI 自主调度了整个搜索和分析过程。

---

## 学术评价

**Feng Zhang**（CRISPR 基因编辑先驱，MIT 和 Broad Institute 教授）审阅预印本后评价：

> "This is an exciting example of how AI agents can contribute to biological discovery. The identification of RNA-repeat arrays associated with reverse transcriptases is genuinely intriguing and merits further investigation. I hope this work encourages more scientists to explore how AI can support their research."

（"这是 AI Agent 如何助力生物发现的激动人心的案例。逆转录酶相关 RNA 重复阵列的发现确实引人入胜，值得进一步研究。希望这项工作鼓励更多科学家探索 AI 如何支持他们的研究。"）

---

## 战略背景

### Anthropic 的生命科学布局

- **2026 年春天**：Anthropic 成立分子生物学研究团队和实验室，验证通用 AI 能否系统性加速基础生物学发现
- **2026 年 4 月**：以 **4 亿美元**收购 Insilico Medicine 的部分资产，加强 AI 制药能力
- 实验室位于湾区，生物安全等级为 BSL-1/BSL-2，不涉及人类病原体
- 所有湿实验由人类科学家完成

### Dario Amodei 的愿景

Anthropic CEO Dario Amodei 在《Machines of Loving Grace》一书中立下宏愿：

> 在 AI 的全面加持下，人类有望在未来 **5–10 年内治愈绝大多数严重疾病**。

此次发现被他形容为"哪怕这是我在读博期间做出的成果，也足以骄傲一辈子"。

---

## ART 会成为下一个 CRISPR 吗？

**目前未知。**

- ART 的生物学功能尚未确定，仍在继续实验中
- 但 ART 具备与 CRISPR 相似的关键特征：逆转录酶 + DNA 重复阵列 + 短 RNA 表达
- 若后续研究证实其可编程性，可能成为新一代基因编辑工具的基础

---

## 核实结果（2026-09-24）

### ✅ 核心事实核实

| 原文说法 | 官方来源核实 |
|---------|------------|
| Claude 发现全新酶系统 ART（Array-Associated Reverse Transcriptase）| Anthropic 官方博客确认 ✅ |
| 950 个 Claude Agent，运行 21 小时，消耗约 2.1 亿 Token | Anthropic 官方博客确认 ✅ |
| ART 存在于噬菌体（bacteriophage）中 | Anthropic 官方博客确认 ✅ |
| ART 由逆转录酶 + 伴侣基因 + DNA 重复阵列组成 | Anthropic 官方博客确认 ✅ |
| 逆转录酶本身此前已被识别，Claude 首次发现其阵列特征 | Anthropic 官方博客确认 ✅ |
| Feng Zhang 审阅后评价"令人鼓舞" | Anthropic 官方博客原文引用 ✅ |
| 实验室位于湾区，BSL-1/BSL-2，所有湿实验由人类完成 | Anthropic 官方博客确认 ✅ |
| ART 阵列会表达出不同的短 RNA（初步实验） | Anthropic 官方博客确认 ✅ |
| Dario Amodei 称"读博期间的成果也值得骄傲" | 多家媒体转引 + Anthropic 官方 tweet ✅ |
| ART 重复阵列含 3–21 个 DNA 重复 | Cryptobriefing 引用官方数据 ✅ |

### 参考来源

- Anthropic 官方公告：https://www.anthropic.com/news/claude-discovers-novel-enzyme-system
- 技术预印本：https://www-cdn.anthropic.com/22573675ada52a8ca8a97a1a4b4326b2f208a071.pdf
- X (Dario Amodei)：https://x.com/DarioAmodei/status/2102831170299834652
- X (AnthropicAI)：https://x.com/AnthropicAI/status/2102824959827742916
- Cryptobriefing：https://cryptobriefing.com/anthropic-claude-discovers-art-bacteriophage-dna/
- NextWeb：https://thenextweb.com/news/anthropic-claude-enzyme-system-crispr-like-repeats
- FourWeekMBA：https://fourweekmba.com/ai-anthropic-claude-agents-art-enzyme-discovery-division-of-lab/

---

*本文为新闻报道整理，不构成任何投资建议。ART 系统的生物学功能仍在研究中，请以 Anthropic 后续正式发表为准。*
