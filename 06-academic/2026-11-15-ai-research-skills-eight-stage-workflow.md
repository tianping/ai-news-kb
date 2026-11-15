# ai-research-skills：17 个科研 Skill 组成八阶段工作流，从开题到投稿（AI学术圆桌派）

- **日期**：2026-11-15
- **来源**：微信公众号「AI学术圆桌派」（作者：小派；文末标注「内容由AI生成」）
- **原标题**：17 个科研 Skill 打包成八阶段工作流，开题到投稿不再反复交代背景
- **分类**：06-academic（科研 Skill / 工作流）
- **URL**：https://mp.weixin.qq.com/s/rV-9BKddEtQhXVEvGGgFgA
- **项目**：GitHub `WenyuChiou/ai-research-skills`
- **相关笔记**：`06-academic/2026-10-20-research-skills-selection-guide-11-projects.md`、`06-academic/2026-10-20-supervisor-skills-hkust-research-skills.md`、`06-academic/2026-10-30-academic-prose-refine-ai-writing-signs.md`

## 一句话

每开一个新的 AI 会话，都得重新讲一遍课题背景、读过的文献和导师的意见。ai-research-skills 把这些写进可移植的 SKILL.md 指令文件，把「文献发现 → 手稿发布」拆成 **8 个阶段、17 个技能**。每个阶段约定好输入、输出和需要人确认的节点。换模型、换客户端，上下文都不会丢。

## 三个场景（附可直接复制的提示词）

### 场景一：读完五十篇文献，还是说不清哪个缺口值得做 → `literature-triage-matrix`（第 2 阶段）

关键在于让模型把方法、数据、主张、局限放进同一张矩阵，原文没写的项就写「未报告」，不让它自己补。

> 用 literature-triage-matrix 处理我上传的 5 篇 PDF。1）逐篇列出研究方法、数据来源、核心主张、明确写出的局限；原文没写的项一律标「未报告」，不要推测。2）输出对比矩阵，列名为 方法／数据／主张／局限，每格不超过 40 字，并附来源文件名与页码。3）矩阵之后单列「跨篇矛盾」与「共同缺口」，每条都必须能回溯到具体某篇的某一页。4）凡是你无法在原文中找到依据的判断，标注「需人工核对」，不要自行补全，也不要生成参考文献。

- 产出的是一张可以和导师逐格讨论的表，不是一段读着顺的总结。
- 配套技能 **notebooklm-brief-verifier**：把 NotebookLM 生成的简报拿回原始材料逐条核对，专门处理「看起来像、其实没出处」的内容。

### 场景二：这个题目还做不做 → `gap-to-topic`（三关判断）

三关是：缺口是否真的还开着；你能否做出可辨识的贡献；现有文献能否支撑论证。

> 用 gap-to-topic 对我给出的候选题目做三关判断。第一关：列出近三年直接相关的 8—12 篇文献，说明该问题是否已被解决；只写能给出处的事实。第二关：写出我可能贡献的具体一点，并列出至少 2 个竞争解释或替代方案。第三关：给出 go／no-go 结论与三条最有杀伤力的反证，结论必须标明「待人工确认」。输出为一份决策档案，禁止把未经检索的判断写成既定事实，禁止编造 DOI、期刊名与年份。

- 它的价值在输出契约：交付的是一份可以被推翻的决策档案，不是一句「这个方向很有前景」。
- 同目录还有 **research-design-helper**：用苏格拉底式追问，把研究问题推到机制、可识别性和验证路径，适合开题前再过一遍。

### 场景三：初稿被说「一股 AI 味」 → `academic-writing-skills`

最常被点名的三处是：术语重复、句式过于整齐、段落之间只靠过渡句硬接。做法是把审计项列死，而不是笼统地让模型「润色」。

> 用 academic-writing-skills 审计下面这一段中文学术文本。1）列出重复出现的术语与套话，给出在原文中的位置，不要直接替换。2）标出连续三句以上结构相同的句式，并给出两版改写建议，保留全部专业术语与数据。3）逐句检查每个论断是否有本段内的证据支撑，无支撑的标「缺证据」。4）不要增删论点，不要改动引文与数字，不要添加总结段；最后单独输出「需作者本人确认」清单。

- 审稿意见回复也算在这个技能里，是高频场景。

## 官方耗时区间

README 原话：这是「维护者观察到的区间，不是基准测试成绩」，实际耗时随模型供应商、网络、文献库状态和提示词写法变化。

| 技能 / 任务 | 耗时 | 轮数 | 备注 |
|---|---|---|---|
| literature-triage-matrix 比较 5 篇 | 约 1—3 分钟 | 1—2 轮 | 随篇数线性增长，20 篇约 5 分钟 |
| gap-to-topic 三关决策 | 约 5—15 分钟 | 3—6 轮 | 随候选题数量和文献回溯深度增加 |
| academic-writing-skills 审计单段 | <1 分钟 | 1 轮 | 与稿件长度无关 |
| academic-writing-skills 回复 6 条审稿意见 | 约 3—8 分钟 | 3—5 轮 | |
| zotero-library-curator 审计 800 条文献 | 约 2—4 分钟 | 1 轮 | 只读 |
| paper-summarize 按主题簇各总结 5 篇 | 约 4—10 分钟 | 1 轮 | 每篇调用一次模型，失败时按篇回滚 |

## 边界：不替你做学术判断

- 不判断创新性，不保证参考文献真实，不替你决定投哪本刊。所有带出处的结论都要回原文核对。
- 中文可以用（FAQ：技能与语言无关，用中文跑就输出中文），但这不代表它理解中文期刊的评价标准。
- **Zotero 写操作风险**：`zotero-library-curator` 只读，只出预览报告；`zotero-skills` 能写库（合并重复、删除条目、重新绑定分类）。官方专门提醒：允许 AI 写 Zotero 之前，先导出完整备份。
- **兼容性**：核心是纯指令的 SKILL.md，Claude Code 插件市场只是方便安装的一层。Codex CLI、Cursor、Windsurf 要把 SKILL.md 放进各自的 skills/rules 目录或直接内联进提示词，Gemini CLI 和通用 API 客户端也一样。README 写明 Hermes 做过 skill-load 验证；**OpenClaw 的目录形状标注为「尚未在此发布验证」**。

## 安装与使用顺序

「30 秒路径」（Claude Code）：

```bash
claude plugin marketplace add WenyuChiou/ai-research-skills
claude plugin install research-workspace@ai-research-skills
claude plugin list   # 确认已启用
```

建议顺序是**先只读，后写入**：

1. 跑 `research-workspace`，建立 `.research` 清单，之后的会话不用再重新扫库；
2. 用 `literature-triage-matrix` 和 `gap-to-topic` 产出矩阵和决策档案；
3. 再进入 drafting 和写作审计；
4. 涉及 Zotero 的写操作放到最后，先备份。

## 责任提醒

- AI 生成的引用、缺口判断、审稿意见回复，都要逐条回原文核对、请导师确认后再用。
- `.research` / `.paper` 里的清单是工作记忆，不是学术证据。
- 用第三方模型或托管服务前，先确认未公开的数据能否上传。
- 技能负责让流程不丢状态，结论的责任始终在作者本人。

## 我的备注

- 文末有「扫码免费领取」引流，原文由 AI 生成；数字和功能以 GitHub README 为准。
- 和 ARS（academic-research-skills）、Supervisor-Skills 同属全流程科研 Skill。这套的特点是每一步都约定好输出契约和人工确认点，并且强制「未报告 / 需人工核对」，不让模型自己补。
- 想在 OpenClaw 里用的话，官方还没验证过目录结构，需要自己把 SKILL.md 放进 skills 目录试一下。
