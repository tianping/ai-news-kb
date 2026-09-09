# Nature Writing Studio：蒸馏 1000 篇顶刊文献的 AI 英文学术写作 Skill

> 来源：微信公众号生信碱移 | 日期：2026-09-09
> 原文链接：https://mp.weixin.qq.com/s/dXVelcLBoTNjGdbnGagS4A
> GitHub：https://github.com/mumdark/nature-writing-studio

## 核心功能

用近千篇 Nature 正刊文章构建蒸馏知识库，将任意文本（中文或英文草稿）改写为顶刊风格的英文。支持摘要、引言、方法、结果、讨论、图注、单句及整篇论文改写。

**四大设计：**

### ① 真实 Nature 文章蒸馏（19,378 种写作模式）
按节抽取真实写作模式，匹配最相似的句式：
- 节首 3-gram：10,488 种（如 abstract 节首最高频 "here we"）
- 节内转折模板：1,668 种（如 intro 中 "Although X, Y"）
- 动词按证据强度分档：124 种（causal > show > suggest）
- 跨节衔接模板：1,766 种（如 "Having established X, we next asked whether Y"）
- results 段开头句：4,864 种（如 "To characterize X, we performed Y"）

### ② 逻辑线提炼
输入文字映射写作逻辑线并显式输出。例：
- 输入：CRISPRi 筛选 287 个转录因子，找到 ZNF219 抑制神经分化
- 输出 text：完整英文摘要级句子
- 输出 logic_line："screen → single hit → mechanism"

### ③ 分模块写作 + Agent 自动识别
不一次性改全文，按论文模块独立改写。7 个 single_section 接口：
| 模块 | 用途 | 建议输入长度 |
|------|------|-------------|
| abstract | 摘要改写 | 100–250 字 |
| introduction | 引言改写 | 300–800 字 |
| methods | 方法改写 | 200–500 字 |
| results | 结果改写 | 300–800 字 |
| discussion | 讨论改写 | 300–600 字 |
| figure_legend | 图注改写 | 50–150 字 |
| sentence | 句级打磨 | 任意 |

另有 `multi_section` 模式：传入整篇草稿，agent 自动识别章节结构、套用版式、跑跨节一致性审计。

### ④ 对抗学习去 AI 味
基于 30 篇真实文章做阴性对照，从 LLM 写作中抽出 AI 常用词并禁用：
- **严重 AI-tell（40 个）**：delve into / navigate the complexities of / shed light on / key role in
- **替换词 AI-tell（127 个）**：leverage / utilize / robust / comprehensive
- **轻微 AI-tell（124 个）**：It's worth noting that / Importantly / It is important to note

## 验收机制

改写后经过质检层，防止捏造信息——伪造数值结果、图片引用、表格引用等。

## 使用方式

在 Codex 等 agent 中安装 Skill 后直接调用：`$nature-writing-studio`。支持单节改写（single_section）和整稿编排（multi_section）两种模式。

---
**标签：** #学术写作 #Nature #Skill #AI写作 #生物信息学
