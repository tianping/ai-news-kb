# claude-obsidian：让 Claude Code 维护 Obsidian 知识图谱（14.4k Star）

- **日期**：2026-10-07
- **来源**：微信公众号「顾北 AI」
- **分类**：02 工具与产品
- **URL**：https://mp.weixin.qq.com/s/JPNqZ6M7aJA6awkl68rIHg
- **项目**：https://github.com/AgriciDaniel/claude-obsidian（MIT，约 14.4k Star / 1.2k Fork）

## 概述

AI 对话产生的想法和分析大多留在聊天历史里，下次遇到同样的问题又得从头来。claude-obsidian 是一个本地优先的知识系统：把文章、PDF、网页、对话等素材交给 Claude Code，加工成带链接、有来源引用的 Obsidian 笔记；之后查询和研究都从这个库里检索。思路是让知识库**每用一次就增值一次，而不是每次重置**。

知识库就是本地的普通 Markdown 加 Obsidian wikilink。不藏在插件缓存里，不锁进云数据库，也不会被悄悄上传给模型。Claude 宕机或以后换了工具，笔记照样能用。

## 核心设计

- **复利循环**：捕获（素材进 inbox）→ 落地（生成带链接的笔记）→ 连接（建立关联）→ 复用（从库里检索答案）。
- **来源溯源**：维护 source ledger（来源账本）和 claim ledger（声明账本），记录每条知识的来源、权威性、新鲜度、支持证据和矛盾证据。高风险声明要求**两个独立来源**支撑；矛盾证据不删，用 `[!contradiction]` 标出来，留给人判断。
- **安全隔离**：产品代码和知识库严格分开。知识库必须用 `CLAUDE_OBSIDIAN_VAULT` 环境变量或配置文件显式指定，不会自动猜目录；URL 抓取等网络请求需要用户明确同意。
- **写入安全**（更接近数据库事务）：
  1. 读取每个目标文件，记下预期的 SHA-256；
  2. 并行的 agent worker 只交草稿和证据，不直接写入；
  3. 全部变更合成一个操作包（operation bundle），由人审查；
  4. 复制操作包的哈希作为授权，执行一次可恢复的事务；
  5. 如果计划生成后文件被改动过，就拒绝写入，不会静默覆盖；中途中断可以用 `transaction recover` 恢复。

## 15 个 Skill（文章列出 11 个）

| 类别 | Skill | 作用 |
|---|---|---|
| 核心 | wiki | 初始化或接管知识库，诊断就绪状态，路由任务 |
| 核心 | save | 主动保存一条有范围的洞察或答案，**不自动记录聊天流水账** |
| 核心 | wiki-ingest | 把 inbox 素材转成带链接的笔记和溯源记录 |
| 核心 | wiki-query | 只读检索，用库里的证据回答问题 |
| 核心 | wiki-lint | 报告死链、孤岛笔记、元数据缺失、过期索引 |
| 扩展 | autoresearch | 有边界的网络研究（3 轮递进），需明确同意联网 |
| 扩展 | canvas | 创建和维护 Obsidian Canvas 知识地图 |
| 扩展 | defuddle | 摄入前清理网页里的广告和噪音 |
| 扩展 | wiki-retrieve | 上下文前缀 + BM25 关键词检索 + 可选的余弦重排序 |
| 扩展 | wiki-mode | 笔记方法论路由：Generic（默认）/ LYT / PARA / Zettelkasten；切换只影响新笔记，不会重组旧笔记 |
| 扩展 | think | 「观察→倾听→连接→创造→成长」结构化思考循环 |

## 上手

环境：Python 3.11+、Claude Code、Bash（Linux / macOS，Windows 需 WSL）；Obsidian 可选但推荐。原生 Windows 只能做只读检查，写入时会报 `UNSUPPORTED_PLATFORM`。

```bash
git clone https://github.com/AgriciDaniel/claude-obsidian.git && cd claude-obsidian   # 产品代码，不是知识库

export GENERATED_AT="$(date -u +%Y-%m-%dT%H:%M:%SZ)" OPERATION_ID="init-reviewed"
# ① 生成计划（只预览）：输出 JSON，含 approved_plan_sha256
python3 scripts/claude-obsidian.py init "$HOME/Documents/MyKnowledgeVault" \
  --generated-at "$GENERATED_AT" --operation-id "$OPERATION_ID"
# ② 审查后带上哈希执行
python3 scripts/claude-obsidian.py init "$HOME/Documents/MyKnowledgeVault" \
  --generated-at "$GENERATED_AT" --operation-id "$OPERATION_ID" \
  --approved-plan-sha256 "<sha256>" --apply
# 已有 Obsidian 库：用 adopt 非破坏性接入

cd "$HOME/Documents/MyKnowledgeVault"
claude --plugin-dir /absolute/path/to/claude-obsidian
# /claude-obsidian:wiki | wiki-ingest | wiki-query | wiki-lint
```

## 评价

- 学习曲线不低。哈希审查第一次用会觉得繁琐，但对存放多年知识的系统来说，这种保守是对的。
- 适合认真经营 Obsidian 库、重度用 Claude Code 做研究或开发、在意数据主权的人。只是偶尔记几条笔记的话，原生 Obsidian 就够了。

## 备注

- 和本机的 `*-kb` 体系思路相近：都是本地 Markdown + git、按主题建库、靠索引互链。claude-obsidian 多了两样：声明账本 + 双来源要求，以及带哈希授权的写入事务。其中 `wiki-lint`（死链、孤岛、过期索引）这类检查可以借鉴到 kb 维护里。
- Star 数和功能描述来自原文，未核实。
