# Claude Code 之父 Boris Cherny 的 CLAUDE.md 模板（13 条）

- **日期**：2026-10-07
- **来源**：微信公众号「零一智源.Ai」
- **原标题**：学习下Claude Code之父的CLAUDE.md模板
- **分类**：02 工具与产品
- **URL**：https://mp.weixin.qq.com/s/axu4y0eQ66LFsiHfeBq5Tg
- **图源**：Charlie Hills 制作的《Anatomy of Boris Cherny's CLAUDE.md》（charliehills.substack.com）

## 概述

`/init` 能根据项目里已有的代码和文档生成 CLAUDE.md。但空项目怎么办？语言风格、自我进化这些也不在 `/init` 的考虑范围里。这篇文章分享了 Claude Code 作者 Boris Cherny 的 CLAUDE.md 结构：左边是原文，右边是 Charlie Hills 的白话解读。另外给了一段 prompt，让 Claude 以这份模板为起点，通过提问帮你写出自己的 CLAUDE.md。

作者提醒：模板偏编程场景，其他场景可以酌情删减。他自己最想用的是 **Self-Improvement Loop**（自我改进循环）。

## 模板全文（据图整理）

### Workflow Orchestration（工作流编排）
1. **Plan Mode Default**：任何非平凡任务（3 步以上）先进 plan mode；跑偏了就**停下来重新规划**。白话：先写计划，免得 Claude 往错误方向冲、白烧 token。
2. **Subagent Strategy**：用子代理保持主上下文干净；一个子代理只做一件事。白话：拆成助手，比如三条调研线并行，主线程保持专注。
3. **Self-Improvement Loop**：**每次被纠正后记到 `tasks/lessons.md`**，每次会话开始时回顾教训。白话：每次修正都写下来，同样的错不犯第二次。
4. **Verification Before Done**：没证明能跑就不许标完成；跑测试、查日志、演示正确性。白话：说「完成」不算完成，要用测试、日志或 diff 证明。
5. **Demand Elegance (Balanced)**：修法感觉像 hack，就实现正规版本；简单明显的修复跳过这步。白话：小改动别过度设计，过度工程也烧额度。
6. **Autonomous Bug Fixing**：报 bug 就直接修，不用手把手；自己看日志、报错、失败测试，然后解决。白话：贴上报错就可以走开。

### 任务管理
7. **Plan First**：计划写到 `tasks/todo.md`，用可勾选的条目。
8. **Verify Plan, Track Progress**：开工前先和你确认；边做边勾。
9. **Explain Changes & Document Results**：每一步给一句话高层摘要；结束时在 todo 里加回顾小节。白话：不搞神秘修改。
10. **Capture Lessons**：每次被纠正后更新 `tasks/lessons.md`。

### 核心原则
11. **Simplicity First**：每处改动尽量简单。白话：一行修复胜过聪明的重写。
12. **No Laziness**：找根因，不打临时补丁。
13. **Minimal Impact**：只动必要的部分，避免引入新 bug。白话：没让重构的别重构，不在无关处搞意外重写。

## 配套 prompt（生成你自己的 CLAUDE.md）

```
Help me build my CLAUDE.md from scratch. Use Boris Cherny's CLAUDE.md as a starting template.
Ask me about my business, voice, banned words, output defaults, and how I want you to work.
Save the final file to ~/CLAUDE.md.
```

注意：这段 prompt 保存到 `~/CLAUDE.md`。要放进当前项目的话，改一下保存路径。

## 备注

- 正文的核心内容在图片里，模板全文是从图片识读整理的。
- 和本库 [Opus 5.5 额度利用](2026-10-07-opus55-subscription-usage-tips.md) 对照看：CLAUDE.md 每轮都会重发，官方建议控制在 200 行以内；这 13 条很精简，符合这个思路。
- 相关笔记：[Claude Code 最强配置：7 层控制体系](2026-09-26-claude-code-strongest-config.md)。
