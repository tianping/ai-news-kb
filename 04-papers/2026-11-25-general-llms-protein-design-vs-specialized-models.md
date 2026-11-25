# 通用大模型开始直接做蛋白质设计：GPT-6 与 Claude 的两条路线，专有模型分工被重写（DeepTech深科技）

- **日期**：2026-11-25（收录）
- **来源**：微信公众号「DeepTech深科技」，原标题「AlphaFold危矣，Claude和GPT都可以直接设计蛋白了，效果还惊人的好」
- **原文**：https://mp.weixin.qq.com/s/Ztrfk_822M_7FPtqlAnd1g
- **分类**：论文与技术突破 / AI for Science
- **一手来源（原文所附）**：
  - Nature 新闻：https://www.nature.com/articles/d41586-026-03836-z
  - OpenAI：https://openai.com/index/gpt-6-models-demonstrate-intrinsic-protein-design-capabilities/
  - Xinming Tu 博客：https://xinmingtu.cn/blog/2026/claude-opus-5-5-protein-sequences/
  - Anthropic 报告 PDF（见原文链接 4）
  - SCMP 关于月之暗面：https://www.scmp.com/tech/article/3374568/
- **相关**：[Claude 自主发现噬菌体酶系统 ART](../01-models/2026-09-23-claude-discovers-art-enzyme-system.md) · [Cell：生成式 AI 在细胞生物学的十五个挑战](2026-09-30-cell-fifteen-challenges-generative-ai-cell-biology.md) · [世界模型的下一个战场：生物医学](../01-models/2026-09-29-world-models-biomedicine-cell.md)

## 一句话

过去几年"通用模型读文献写代码、专有模型（AlphaFold/RFdiffusion/ProteinMPNN）做结构预测与分子设计"的分工正在松动：通用大模型开始直接参与蛋白质设计环节，并有湿实验数据支撑；但在从头预测完整结构这类任务上仍明显落后于专有系统。

## 事情的起点

- 11 月中旬，AI 生物公司 Phylo 研究员 Xinming Tu 测试 OpenAI、Anthropic、Google 的通用模型预测冷门细菌蛋白序列；最新版 Claude 不仅胜过其他通用模型，也超过了专门用海量序列训练的蛋白质语言模型
- 他与哈佛医学院计算生物学家 Leo Chen 把结果发在博客上，在计算生物学圈引发一轮测试热潮；后续发现 Claude 还能预测接触图（contact map），但精度仍不及专有模型 ESM-2

## 两条技术路线

**OpenAI：模型自己推理 + 写代码**
- 不是凭空生成坐标，而是先结合数学、物理、化学知识推理相互作用机制，再自主写代码探索三维构象
- 官方报告给出四类任务的计算与湿实验结果（骨架生成、迷你结合蛋白、反向折叠、抗体改造），部分指标高于 RFdiffusion、Proteina 等专有基线
- 小分子对接评估中，与实验结构的匹配率 92%–97%，明显高于 DiffDock（60%）、DiffDock-L（50%）
- OpenAI 自己点明两点局限：湿实验流程里模型只负责提出三维骨架，序列填充和打分仍要调用 SolubleMPNN 等专有模型；从头预测完整结构仍明显落后 AlphaFold
- 还提醒：湿实验里"能结合"只是早期信号，不等于具备成药所需的特异性、稳定性或生物活性
- 下一步计划：更难的靶点、从"能结合"走向"产生特定生物学效应"，并扩展到小分子、RNA、PROTACs

**Anthropic：通用模型当智能体调度现有工具**
- 比 OpenAI 报告早三个月；把标准流程写成协议提示词，让 Claude Opus 4.8 和 Mythos Preview 无人干预连续运行 24–48 小时
- 模型自主完成靶点调研、表位挑选，并自行安装调度 7 种以上开源蛋白设计与结构预测模型，输出排序后的候选
- 由 Twist Bioscience 与 Adaptyv Bio 两家独立机构做湿实验验证，1320 个设计中整体命中 27%（354/1320）；15 个有效读数靶点中 14 个有结合设计；各靶点排名第一的设计命中率 49%
- 其中 RBX1（E3 连接酶亚基）曾是公开设计大赛靶点：人类参赛者 245 个设计仅 9 个结合、冠军亲和力 45 nM；Claude 90 个设计中 28 个结合，最强 3.9 nM

**国内**：月之暗面把科研列为编程之后的下一个方向，走"工具调度"路线——Kimi 接入深势科技的 BioMaster 负责理解问题、写代码、调工具；与晶泰科技合作从化学表达式和图表中提取信息。整体仍处早期，集中在文献处理和工作流辅助。

## 专有模型会被淘汰吗

- **组织层面的信号**：今年 7 月 Google 解散重组原 AlphaFold 团队，多数成员转去做 Gemini，其余流向 Isomorphic Labs 或离职
- **资本层面的反向信号**：Isomorphic Labs 第三轮融资 50 亿美元；逻辑是生成结合蛋白只是第一步，毒性、代谢、成药性优化高度依赖药企与湿实验室的私有数据，瞄准的是真实研发管线而非公开基准
- **数据差距**：通用模型主要学公开论文和互联网语料，而学术界发表的是筛选过的成功案例，失败实验、放弃的方案、调整方向的判断依据都不进公开数据集
- 因此通用模型公司也在补实验短板：Anthropic 在旧金山湾区设生物湿实验室，Google DeepMind 在英国弗朗西斯·克里克研究所建湿实验室；但涉及商业机密的核心数据仍在药企和垂直公司手里

**业内分歧**
| 观点 | 人物 |
|---|---|
| 乐观："大语言模型技术上可以在每一项能力上击败专有模型" | Masatoshi Uehara（参与 OpenAI 研究） |
| 质疑："这些报告并不令人印象深刻，展示的用例用专有模型本来就处理得更好"，通用模型要证明的是能解决专有工具做不出来的难题 | Martin Pačesa（苏黎世大学结构生物学家） |
| 开放："可能是死胡同，也可能是更大变革的开端" | Jan Kosinski（EMBL 结构建模专家） |

## 点评

- 标题"AlphaFold 危矣"偏激：OpenAI 自己承认从头预测完整结构仍落后 AlphaFold，且其湿实验流程仍要靠专有模型补齐序列和打分，是混合流程而非替代
- 更站得住的判断是文末那句：只靠公开数据集刷分的专有模型空间会变小；当通用模型能在 48 小时内批量产出候选，单纯的生成能力不再稀缺，价值转移到真实实验数据和下游成药环节
- 命中率数字来自各家自己公布的报告（Anthropic 的由两家第三方机构做实验验证），尚无独立复现；"能结合"离"能成药"还很远
- 本笔记只记录公开报道的结论与数据，不含实验方法细节
