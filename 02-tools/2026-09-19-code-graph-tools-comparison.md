# 代码图谱工具怎么选:graphify / codegraph / serena / repowise 四选一

## 元数据
- 日期：2026-09-19
- 来源：沉渊说（公众号）
- 分类：02-tools（AI 编程/代码图谱）
- URL：https://mp.weixin.qq.com/s/mmgoU9DlU4qDeQ2Dz9Ba1g

## 概述
对比四个把"理解代码"变成可索引产物的活跃项目：graphify、codegraph、serena、repowise。它们走三条不同路线——图派（预建图谱）、符号派（按需查 IDE 符号）、情报派（除代码外还把 git 历史/测试/健康度当证据）。数据口径：GitHub API 实时抓取于 2026-09-19。

## 四项目速览

| 项目 | Star | 授权 | 形态 | 它给什么 |
|------|------|------|------|----------|
| graphify | 119,308 | Apache-2.0 | Python | 任意语料(代码+文档+PDF+图片)变可查询知识图谱，Claude Code 技能分发 |
| codegraph | 71,414 | MIT | C+Rust 内核 | 预建代码图谱，改代码自动同步，给 9 种 agent 当"地图" |
| serena | 29,571 | GPL-3.0-or-later | Python | 一套 MCP 工具，让 agent 像用 IDE 一样按符号找/改代码 |
| repowise | 6,666 | AGPL-3.0 或商业 | Python/TS | 一次索引出图、git、文档、决策、健康度五层 |

- 建仓时间：graphify 2026-04-03、codegraph 2026-01-18、serena 2025-03-23、repowise 2026-03-23。
- 最近提交均在 5 天内(repowise/serena 09-18，graphify/codegraph 09-16)。
- 热度：graphify 9月11日 116,925★ → 9月19日 119,308★，8 天涨约 2,400 星。

## 三条路线

1. **图派（graphify / codegraph）**——提前把结构算好存成图，agent 来查。查询快、跨文件视野全；代价是图必须维护，准确率直接决定收益（codegraph 开发者：认错一次，agent 就去读一堆无关文件，省 token 收益归零）。

2. **符号派（serena）**——不预建图，按需问语言服务器（LSP）或 JetBrains 引擎。永远和代码同步、能做精确重构（跨文件重命名/移动/安全删除一次原子调用）；代价是免费后端无跨语言全局视图、能力受各家 LSP 限制。

3. **情报派（repowise）**——图谱只是第一层，真正卖的是把 git 历史和测试也当证据。能回答静态分析答不了的问题；代价是最重(125MB 级别)、最年轻、星最少、商用需留意 AGPL。

## 各项目要点

### graphify（唯一多模态）
- Claude Code 里敲 `/graphify .`，读目录下所有东西，用 Claude 视觉抽取概念关系连成图。
- 分工：代码走本地 tree-sitter AST（确定性），仅文档/图片/PDF 交 Claude（花钱）。
- 每条边带 EXTRACTED / INFERRED / AMBIGUOUS 诚实标记；gob nodes 与 surprising connections 分开列。
- 输出：graph.html(交互图)、obsidian/(可直接开 Obsidian)、--wiki(每社区一篇 Wikipedia 风文章+index.md)。
- 收益随语料变大：作者跑分 52 文件混合语料"每次查询少 71.5 倍 token"，6 文件语料约 1x。
- 代价：强绑 Claude Code、1,400 未关 issue。

### codegraph（工程化最完整）
- 一条命令安装，自动探测配置 9 种 agent 的 MCP 入口；`codegraph init` 后自动同步默认开启，索引落 `.codegraph/codegraph.db`(SQLite+FTS5)。
- Rust 内核，tree-sitter 编译进；20 种语言完整支持，其余走 portable 引擎。
- 只暴露 1 个 MCP 工具 `codegraph_explore`（作者：一个强工具比一排窄工具更能引导 agent）；另 7 个工具默认隐藏。
- 官方 2026-08 复测：7 个 benchmark 仓库平均成本低 44%、token 少 62%；28-43 次工具调用的问题省 57-78%，7 次内基本打平。作者限定：成本跟提问需多少探索有关，非仓库大小。
- 代价：只懂代码；遥测默认"询问"；版本节奏慢一档(v1.6.0 8月26日)。

### serena（把 IDE 能力借给 agent）
- 不预建索引，按需用 IDE 符号级能力问代码；自称"你的编码 agent 的 IDE"。
- 两类后端：免费 LSP(默认) 与付费 JetBrains 插件(有试用)。差别：类型层级/依赖搜索/移动符号/交互式调试仅 JetBrains 有。
- LSP 后端覆盖 40+ 语言(含 GDScript、Lean 4、Solidity、SystemVerilog)。
- 宣传点在"更可靠"非"省 token"：无精确重构工具 agent 只能靠不靠谱搜索替换；有之后跨文件改名/移动成一次原子调用。
- 评测：让 agent 真实仓库跑约 20 个任务引用原话(Opus 4.6 在 Claude Code)："跨文件重命名、移动和引用查找从 8 到 12 步易错操作塌缩成一次原子调用"。
- 带轻量记忆系统(常与 AGENTS.md 配合)+ 分层配置(四个里最细)；官方提醒别从 MCP/插件市场装(命令已过时)。

### repowise（把代码之外"考古证据"也索引进去）
- 一次索引五层：图(26 种语言 AST+带置信度调用解析)、git(热点/所有权/巴士因子/修复历史)、文档(每模块每文件 wiki 增量重建带新鲜度与置信度)、决策(5 索引源+人工/agent 捕获，每条能追证据)、健康度(49 个确定性检测器带重构计划)。
- "零 LLM 调用"：图、风险、健康度、测试推荐、死代码、PR 分析全不需要模型；只有"散文"和注释考古要 provider。
- 10 个 MCP 工具按"任务"而非"数据实体"设计。distill 功能：压掉命令输出噪声(错误优先、保留退出码、[repowise#ref] 标记可还原)，如 distill pytest 省 61% token 且 11 行失败信息一行不丢、git log -50 省 89%。
- 覆盖率报告(LCOV/Cobertura/Clover)；无报告时用调用图回答"改这个文件该跑哪些测试"。
- 代价：AGPL-3.0 或商业授权、125MB 最重、星最少。

## 作者实测（仅 repowise）
- 对象：49 文件、4.9MB Godot 仓库副本(放 /tmp)。命令 `repowise init --no-prose -y`。
- 结果：建索引 11.8 秒；消耗 token 0(全程未配 provider)；索引体积 3.4MB/67 wiki 页；图谱 545 节点/734 边(defines 488、calls 122)；识别 gdscript/godot_resource/python/json/markdown/shell；git 覆盖 32/32 文件 11 commit；健康分 9.57/10(最差一 119 行关卡脚本 7.84)；死代码 5 处；distill git log -20 压成 11 行标题省约 1,408 tokens。
- 观察一：GDScript 真被解析(2026 年 3 月建仓工具已支持游戏引擎语言)。
- 观察二："零 LLM"非宣传话术(token 计数确实 0)，wiki 页是结构化模板非散文(要散文开 --prose)。

## 一句话选择
- 纯代码任务、少翻文件 → **codegraph**（20 语言、编辑即更新、零外部模型、MIT、自包含二进制）
- 混合语料(论文/设计稿/白板照片) → **graphify**（唯一多模态，强绑 Claude Code）
- 跨文件重命名/移动/删死代码重构 → **serena**（唯一走 LSP/JetBrains 符号路线，能原子改代码）
- PR 风险/该跑哪些测试/死代码/健康分工程指标 → **repowise**（零 LLM 调用；商用勿碰 AGPL）
- 不想装服务/常驻进程 → codegraph（自带运行时自包含构建）

## 未验证项（作者声明不作结论主张）
- 其余三家 token/成本收益均为厂商自测未复现；图谱准确率未人工抽检；codegraph await 误解析修复为更新日志口径转述；serena 评测为厂商组织；十万文件级大仓库索引耗时未测；授权差异不做法律意见。

## 影响与备注
- 反常识：星数与"对你的代码任务多合适"非正相关——graphify 11.9万星很大部分价值在非代码语料；纯源码仓库反而 7.1 万星的 codegraph 更对症。
- 推荐组合：codegraph/graphify 管"让 agent 去哪读"，serena 管"让 agent 敢动手改"，repowise 管"合并前告诉你哪里危险"。真正要避开的是把图谱当答案本身——图错了省下的 token 全在返工里还回去。
- 与本库 AGENTS.md(Anthropic 接受 OpenAI 标准) 笔记互链：项目指令文件已成代码库基础设施，图谱类工具与之互补。