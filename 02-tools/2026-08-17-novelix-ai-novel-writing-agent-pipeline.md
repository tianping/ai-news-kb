# Novelix：10 Agent 写审改流水线 + 33 维连续性审计，去 AI 味过朱雀

> 来源：[AI写小说怎么去除AI味？Novelix：10个Agent写审改，33维连续性审计，朱雀可过](https://mp.weixin.qq.com/s/3CMY5p3pEdLBMe_aGW-Qfw) · 2026-08-17

---

## 项目概览

- **GitHub**：https://github.com/zxerai/novelix
- **npm 包**：@actalk/novelix
- **协议**：AGPL-3.0
- **语言**：TypeScript 99.5%
- **定位**：AI Agent 自主写小说
- **核心理念**：不靠超长上下文，而是多 Agent 分工协作 + 状态文件管理

---

## 核心架构：10 Agent 管线

每写一章，10 个 Agent 按三阶段接力：

### Phase 1：创意写作（温度 0.7）
| Agent | 职责 |
|-------|------|
| 规划师 | 读作者意图和当前焦点，产出本章意图和 hook 议程 |
| 编排师 | 从真相文件中按相关性选择上下文（只取本章需要的角色状态、伏笔、支线进度） |
| 写手 | 生成正文，内置字数治理、第一屏 hook、语义密度控制、hook 兑现、移动端段落节奏 |

### Phase 2：状态沉淀（温度 0.3）
| Agent | 职责 |
|-------|------|
| 观察者 | 从刚写章节提取 9 类事实：角色、位置、资源、关系变化等 |
| 反射器 | 输出 JSON delta（非全量重写），Zod schema 校验后 immutable 写入真相文件。伏笔用 upsert/mention/resolve/defer 四种语义操作 |

### Phase 3：质量循环
| Agent | 职责 |
|-------|------|
| 归一化器 | 检查字数偏差，超出范围单 pass 纠偏 |
| 审计员 | 跑 33 维连续性检查 |
| 修订者 | 自动修复审计发现的问题，自纠循环直到关键问题清零 |

**流程图**：
```
规划师 → 编排师 → 写手（Phase 1）
          ↓
观察者 → 反射器（Phase 2）
          ↓
归一化器 → 审计员 → 修订者（Phase 3，可配置 1-3 轮）
```

---

## 7 个真相文件：不靠记忆靠账本

| 文件 | 用途 |
|------|------|
| `current_state.md` | 世界状态：角色位置、关系网络 |
| `particle_ledger.md` | 资源账本：物品、金钱、衰减 |
| `pending_hooks.md` | 伏笔池：铺垫、承诺、未解决冲突 |
| `chapter_summaries.md` | 各章摘要：出场人物、事件 |
| `subplot_board.md` | 支线进度板 |
| `emotional_arcs.md` | 按角色追踪情绪变化 |
| `character_matrix.md` | 角色交互矩阵、信息边界 |

写手不靠「记住」前文，而是每次从真相文件读取当前状态。主角不会凭空想起没见过的事，也不会拿出两章前丢掉的武器。

> Node 22+ 自动启用 SQLite 记忆数据库，按相关性检索历史事实，进一步降低 token 消耗。

---

## 33 维连续性审计

审计员每章对照 7 个真相文件检查 33 个维度，包括但不限于：

- 主角性格是否前后一致
- 配角的知情边界是否合理（不该知道的事有没有知道）
- 物品、金钱是否有据可查
- 伏笔是否按时回收
- 支线进度是否合理推进
- 叙事节奏是否过快或过慢
- 情感弧线连续性
- 角色记忆一致性
- 物资连续性
- 大纲偏离检测

发现问题后，修订者自动修复，默认最多 1 轮，可配置到 3 轮。

---

## 去 AI 味：22 条规则，朱雀可过

`novelix revise --mode anti-detect` 内置 22 条改写规则：

| 类别 | 条数 | 说明 |
|------|------|------|
| 句式变异 | 5 | 打破 AI 的均匀句长，制造长短交替的节奏感 |
| 词汇替换 | 5 | 消灭「仿佛」「不禁」「宛如」「突然」等 AI 高频词，降低「了」字频率，替换模板化表达 |
| 段落呼吸 | 3 | 调整段落长度分布，避免 AI 标志性的均匀段落 |
| AI 标志表达消除 | 4 | 移除过度修饰、完美过渡等 AI 写作特征 |
| 朱雀专项 | 5 | 句长波动、标点多样性、代词降频、避免完美过渡、副词降频——专门针对朱雀检测器判定逻辑优化 |

内置 AI-tell 检测词表覆盖：
- 17 个模糊词
- 22 个过渡词
- 20 个描写词
- 10 个叙述词
- 15 个题材各有专属疲劳词表

---

## 15 个题材，中英双语

### 5 个中文网文题材
玄幻、仙侠、都市、恐怖、其他（自定义）。每个题材有专属节奏规则、疲劳词表和审计维度。

### 10 个英文网文题材
LitRPG、Progression Fantasy、升级流、异世界、修仙（英文）、系统末日、地牢核心、浪漫奇幻、科幻、爬塔、治愈奇幻。

英文题材也有专属疲劳词表，比如 LitRPG 圈公认的 AI 高频词「delve」「tapestry」「testament」会被标记。

---

## 4 种同人模式

| 模式 | 说明 |
|------|------|
| canon | 忠实原作，严格保持正史一致性 |
| au | 平行宇宙，可改设定 |
| ooc | 角色崩坏，可偏离原角色性格 |
| cp | 配向/CP 向 |

---

## 3 分钟上手

```bash
# 1. 安装
npm i -g @actalk/novelix

# 2. 初始化项目
novelix init my-novel

# 3. 检查配置
novelix doctor

# 4. 创建书籍
novelix book create --title "吞天魔帝" --genre xuanhuan

# 5. 一键写 10 章
novelix write next 吞天魔帝 --count 10
```

打开 Studio Web 工作台：终端输入 `novelix`，浏览器访问 http://localhost:4567

---

## 多模型路由：贵的给写手，便宜的给审计

支持 15+ LLM 服务商：OpenAI、Anthropic、DeepSeek、月之暗面、智谱、百炼、MiniMax、Ollama（本地）、OpenRouter 等。

不同 Agent 可以用不同模型，按需平衡质量和成本：

```bash
# 写手用 Claude（创意强）
novelix config set-model writer claude-sonnet-4-20250514 --provider anthropic

# 审计员用 GPT-4o（快速便宜）
novelix config set-model auditor gpt-4o --provider openai

# 雷达用本地 Ollama（零成本）
novelix config set-model radar qwen2.5:14b --provider ollama
```

未显式覆盖的 Agent 回退到全局模型。

---

## Studio Web 工作台

- **书籍管理**：创建、审阅、导出（txt/md/epub）
- **数据分析**：审计通过率环形图、高频问题排名、Token 用量统计、章节字数趋势
- **知识图谱**：角色关系力导向图，60fps 可拖拽缩放，SVG 箭头指示，玻璃态样式
- **Chat 工作台**：自然语言交互，内置 18 个工具

---

## 其他实用功能

- **文风仿写**：`novelix style analyze` 提取参考文本统计指纹（句长分布、词频、节奏），`novelix style import` 注入书籍，后续章节自动采用该风格，风格规则进入审计标准
- **续写已有小说**：`novelix import chapters` 导入已有小说，自动逆向 7 个真相文件 + 生成风格指南，无缝续写。支持单文件按章节切分、目录批量导入、断点续传
- **独立短篇包**：`novelix short run` 一键产出短篇，输出 `full.md`（全文）、`sales-package.md`（营销包）、`cover-prompt.md`（封面提示词）、`cover.png`（封面图）
- **全书改名**：`/rename 林烬 => 张三`，一次扫描所有章节 + 真相文件，避免漏改
- **AIGC 检测**：11 条确定性规则零成本检测，可选 LLM 校验

---

## 22 条命令速查

| 命令 | 用途 |
|------|------|
| `novelix init` | 初始化项目 |
| `novelix book create` | 创建新书 |
| `novelix write next` | 完整管线写下一章 |
| `novelix draft` | 只写草稿 |
| `novelix audit` | 审计 |
| `novelix revise --mode anti-detect` | 去 AI 味修订 |
| `novelix agent` | 自然语言模式 |
| `novelix import chapters` | 导入续写 |
| `novelix fanfic init` | 创建同人书 |
| `novelix style analyze` | 文风分析 |
| `novelix export --format epub` | 导出 EPUB |
| `novelix short run` | 写短篇 |
| `novelix doctor` | 诊断配置 |
| `novelix studio` | 启动 Web 工作台 |

---

## 小结

Novelix 把 AI 写小说从「一个 LLM 写到底」升级为「多 Agent 流水线 + 状态管理 + 质量审计」。10 个 Agent 分工协作，7 个真相文件管状态，33 维审计查连续性，22 条规则去 AI 味。支持 15 个题材、4 种同人模式、15+ LLM 服务商，npm 一键安装本地跑。

项目 2026 年 6 月初创，比较新，适合愿意尝鲜的网文写手和 AI 写作工具开发者。