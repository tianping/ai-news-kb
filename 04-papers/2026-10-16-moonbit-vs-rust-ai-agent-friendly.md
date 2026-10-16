# MoonBit vs Rust：同一个 SQLite 引擎让 AI 各写三遍，MoonBit 少约 12% token（CSDN）

- **日期**：2026-10-16
- **来源**：微信公众号「CSDN」（原标题：超越 Rust，MoonBit 正成为对 AI 更友好的语言）
- **分类**：04 论文与技术突破
- **原文**：https://mp.weixin.qq.com/s/M1blFy8Ipm8Y39OpMuPmQQ
- **实验仓库**：https://github.com/bobzhang/minisql-moonbit-vs-rust （预印本《Agent-Friendly by Design》在 `paper/` 下）

## 概述

讨论「哪种编程语言更适合 AI Agent 写代码」。引子：开发者 Mirek Rusin（2026-10-08，X）做内存型 SQL Server 模拟器 **bitsql**，TypeScript+SQLite → Rust+DuckDB 十多天未完成，换 MoonBit 几小时搞定（三次技术栈不同、对项目理解逐步加深，不能直接推算效率差）。核心证据是一项公开对照实验：同一 Agent 用 Rust 和 MoonBit 各三次从零写 SQLite 兼容数据库引擎。结论：**MoonBit 源码 token 少约 12%，隐藏测试通过率和成本接近 Rust**；更值得注意的是 MoonBit 语料远少于 Rust 仍能做到接近。

## Rust 的两个痛点（对 Agent 尤其明显）

- **编译等待**：Rust 官方编译性能调查（3700+ 受访者）中 55% 表示最困扰的项目改代码后重建超过 10 秒；Agent 每轮都要等编译+测试，几百轮累积可观
- **磁盘占用**：2025 Rust 年度调查中构建产物过大是仅次于编译速度的第二大问题；编译 Cargo 自身 dev 配置 target 目录约 3.08 GiB；多个 Agent 各自工作目录无法共享缓存会占多份

## 实验设计（minisql-moonbit-vs-rust）

- 模型：**Claude Opus 5.5**，经 Claude Code 运行，推理强度 high
- Rust / MoonBit 各 3 次独立构建，每次 9 个开发阶段（SQL 解析、查询执行、事务、窗口函数、按 SQLite 公开规范读写数据库文件：页结构、B-tree、变长整数、记录编码），最终约 1.3 万行
- 489 个测试用例，预期结果由 SQLite 3.53 生成；其中 **242 个隐藏测试**始终不对 Agent 开放，用于最终评估
- 控制变量：相同规格、测试、模型、提示词结构、工具，成对并发；只许用标准库，禁第三方包与自行 FFI；固定 I/O 层（MoonBit 侧预置 C stub 不可改）；每阶段新会话，重读代码开销计入

## 结果

| 指标 | 结果 |
|---|---|
| 最终源码 token | MoonBit 少约 12%（行数反而略多：13,842 vs 13,298） |
| 累计输出 token | MoonBit 少约 12%；9 阶段中 8 个更低（连接/子查询 0.76，索引/视图/事务 0.85） |
| 隐藏测试通过率 | 相差约 0.7 个百分点（每次构建不到 2 个测试），3 次样本不足以分高下 |
| 成本 | 均值 MoonBit ≈ Rust 的 0.94 倍，中位数却 ≈ 1.12 倍；Rust 三次 $35.29—$59.12，波动大于语言差 → 基本持平 |
| 编译检查失败率 | MoonBit 13.0%，约 Rust 的 2.25 倍 |
| 类型检查耗时 | MoonBit 约 0.2 秒；Rust 干净 `cargo check` 约 0.8—1.1 秒 |
| 运行速度 | 10 项微基准几何平均，MoonBit 耗时约为 Rust 的 1.25 倍 |
| 峰值内存（索引负载） | MoonBit 170 MB vs Rust 601 MB |

### 为什么 MoonBit 更紧凑
1. **自动内存管理**：单线程数据结构任务，无需借用、生命周期、共享所有权标注
2. **错误处理**：函数签名用 `raise` 声明错误，省去调用处反复 `?`；抽查的 SQLite 文件读取器 token 少约 39%（含借用/生命周期等多因素，且为「非注释字符数÷4」粗估）
3. **反馈快**：检查 0.2 秒，语言指南鼓励每次编辑后 `moon check`，Agent 改一点查一次

### MoonBit 暴露的问题
- 约一半检查失败来自通用开发问题（没写完就检查、重构连锁修改、新增枚举分支）
- 语言相关错误集中在少数模式：把 `using` 当变量名、把 Rust 的 `let (mut a, b)` 带进来、猜测不存在的 API、错误效果标注；估算每次构建额外约 11 次 API 调用、0.43 美元
- 工具链细节：Agent 常用 `tail` 截编译器输出，长警告把真正的错误挤出截取范围 → 错误信息可读性、API 可查性也影响 Agent 效率

### 成本的隐藏变量：Agent 什么时候停手
- **47%—82% 的输入 token 消耗在可见测试已全部通过之后**（差分测试、性能分析、边界情况）
- 只看首次全绿前，MoonBit 约为 Rust 的 1.09 倍 → 比较 AI 开发成本必须先约定「何时算完成」

## 旁证：低资源语言也能写好

- 论文《No Resource, No Benchmarks, No Problem?》（Giagnorio、Martin-Lopez、Bavota，2026-06，arXiv 2606.16827）：截至 2025-07-02，MoonBit 约 400 个公开 GitHub 仓库，Gleam 约 2,900（约 1/7），Rust 百万级
- 微调数据 MoonBit 0.5M token、Gleam 3.6M token（约 1/7），两个 Qwen 模型微调后 MoonBit 在三个基准上 pass@1 都更高；给少量示例或检索文档时 MoonBit 提升也更大
- 作者推测与语言面向 AI 设计有关，但未证明因果，也非所有设置都领先；函数生成 ≠ 数小时工程开发，分数不可横比

## 生态

- Mooncakes 包仓库 27,108 个包，约为年初（约 2,500）的 10.8 倍
- 2026 MoonBit 黑客松进行中：https://moonbitlang.github.io/Hackathon2026/

## 备注

- 局限（原文自述）：只用一个模型、每种语言 3 次、任务限于无第三方库的单线程数据库引擎；运行速度与内存各有优劣，不能外推到其他工程场景。
- 本文明显带推广性质（结尾为黑客松宣传）；实验仓库 `bobzhang` 应为 MoonBit 发起人张宏波的账号，即实验由语言方主导，结论宜待第三方复现。（作者身份为据仓库名推断，未核实）
- Rust 相关数据出处：Cargo 团队博客（2026-08-18 减小 target 目录实验）、Rust 2025 编译器性能调查（2025-09-10）。
