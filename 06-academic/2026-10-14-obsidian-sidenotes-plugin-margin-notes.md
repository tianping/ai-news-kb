# Obsidian Sidenotes 插件：图夫特风格"书边批注"，不打乱正文的读书 / 文献笔记

- **日期**：2026-10-14
- **来源**：微信公众号「obsidian指南」（作者小宅）
- **分类**：06 AI 学术应用（科研工具链 / 文献阅读）
- **原文**：https://mp.weixin.qq.com/s/MLK2saMMdXhikiN2abn75A

## 概述

传统脚注要么在页面底部、来回滚动，要么在正文里插大段注释打乱结构。**Sidenotes**（原名 Side-Notes，作者 Fried Fishsticks）把注释显示在**正文右侧空白处**，复现纸质书旁注体验；设计理念借鉴 Edward Tufte 的信息设计原则（Tufte 式 sidenote）。

- 阅读模式：注释显示在文章右侧边缘
- 编辑模式：正文里只有一个小标注符号
- 实时编辑：直接在右侧批注上修改，回车保存

## 与原生脚注对比

| | 原生脚注 | Sidenotes |
|---|---|---|
| 显示位置 | 页面底部 | 正文右侧边缘 |
| 阅读体验 | 上下来回滚动 | 视线横向平移 |
| 排版 | 打乱结构 | 分栏布局 |
| 查看批注 | 手动跳转 | 直接阅读 |

关键区别：脚注把注意力从内容抽离；边注让思绪保持在同一视觉平面，减少认知负担。

## 安装

设置 → 社区插件（关闭安全模式）→ 搜索 **Sidenotes**（作者 Fried Fishsticks）→ 安装并启用。

## 两种用法

1. **命令面板插入**（新手）：选中要标注的内容，`Ctrl/Cmd + P` → 输入 `Insert sidenote` 回车。⚠️ 开着侧边栏时要把侧边栏往右拉窄些，否则主页面太窄不显示边注。
2. **Markdown 脚注转换**（兼容老笔记）：设置里开启 `Use Markdown footnotes`，沿用标准语法，插件自动渲染到右侧：
   ```markdown
   正文内容[^1]

   [^1]: 这是你的边注内容
   ```
   勾选 `Hide footnote numbers` 可隐藏正文里的 `[^1]` 编号。

## 关键设置

| 设置项 | 说明 | 推荐 |
|---|---|---|
| Sidenote Format | HTML 模式 / 脚注模式 | 新笔记 HTML，老笔记脚注 |
| Hide footnotes at bottom | 隐藏底部原始脚注 | 开启 |
| Hide footnote reference numbers | 隐藏正文编号 | 开启 |
| Number Style | 数字或新拟态徽章 | 默认数字 |
| Minimum / Maximum Sidenote Width | 边注宽度 | 300px / 420px（笔记本建议 ≤350px） |
| Sidenote Anchor | 锚点跟随模式 | Anchor to text（最稳定） |

## 适用场景

1. **读书笔记与文献精读**（最推荐）：核心概念解释、个人理解、相关案例批注在段落旁
2. **长文写作与修订**：补充说明、参考案例、版本对比放边注，保持主文流畅
3. **概念分级批注**：右侧解释概念 A，A 里再嵌套概念 B 的解释
4. **技术文档**：代码示例、API 参数、版本兼容信息放边注

作者的用法：左边原文理解，右边个人思考、案例补充、交叉引用。

## 备注

- 纯排版/阅读插件，不涉及 AI；放在本分类是因为主要用于文献精读。"阅读效率提升 30%"为作者主观估计。
- 相关：本库 [科研人的 Obsidian 实战手册（第 1 期）](2026-10-14-obsidian-research-workflow-series-01.md)、[Claudian 插件](../02-tools/2026-10-14-claudian-obsidian-agent-plugin.md)、[obsidian-skills](../02-tools/2026-10-14-kepano-obsidian-skills-agent-guide.md)。若让 AI 生成带边注的笔记，脚注模式下 AI 写标准 `[^1]` 语法即可被渲染。
