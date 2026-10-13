# REA + Reverse-skill + PentAGI：让 Codex 1:1 复刻任何软件功能（草稿鸭）

- **收藏日期**: 2026-10-13
- **来源**: 微信公众号「草稿鸭」
- **分类**: 工具与产品 / AI 编程 Agent / 逆向分析
- **原文链接**: https://mp.weixin.qq.com/s/dK_p1fHAQA3u-zbn5er7ZA
- **相关笔记**: 本库 [REA（Reverse Engineer Anything）](2026-10-12-rea-reverse-engineer-anything.md)、[用 REA 逆向复刻 Microsoft Word（杉森楠）](2026-10-13-rea-reverse-engineer-microsoft-word.md)

## 概述

REA 近期在 X 刷屏，但"逆向 ≠ 复刻"。作者把三个开源工具接成一条流水线交给 Codex（或 Claude Code）：看到喜欢的功能 → 搞懂怎么实现 → 用自己的技术栈重写 → 上线前做安全体检。面向不懂编程的小白，用户只负责说清需求和验收。

## 三个工具

| 工具 | 仓库 | 角色 |
|------|------|------|
| REA（Reverse Engineer Anything） | github.com/morluto/rea | 通过 MCP 接入 Codex/Claude Code，读网页、桌面应用、二进制程序本身及其行为，追踪处理过程，给出**有出处的解释**（每条结论附文件、代码段依据 = Evidence 记录） |
| Reverse-skill | github.com/zhaoxuya520/reverse-skill | 技能路由包（支持 Claude Code/Cursor/Cline/Kiro）：AI 自动判断走哪条分析线；按需自举工具链（jadx、apktool 等现用现装）；自动进化经验库，记录踩坑下次绕开 |
| PentAGI | github.com/vxcontrol/pentagi | 开源"AI 红队"：多智能体分工（搜信息/写测试代码/执行），带二十多种安全工具，全程在 Docker 沙箱跑，输出漏洞报告 |

分工一句话：REA 帮你看懂，Reverse-skill 帮你调度，PentAGI 帮你体检，Codex 串起来动手做。

## 环境准备

- 代码 AI 客户端（Codex 或 Claude Code 桌面端）+ Node.js
- 只有拆手机 App 才需要 Java
- PentAGI 需要 Docker，内存 4G 以上

## 五步工作流（以复刻网页小功能为例）

1. **装工具**：把"读这两个仓库的 README，按我的系统（Windows/macOS）一步步带我接入，装好后给一句测试指令"发给 Codex，让它装 REA 和 Reverse-skill。
2. **让 REA 读懂目标功能**：如"Chrome 断网小恐龙的障碍物为什么越来越快"——要求先用 Reverse-skill 选分析线、补工具，再用 REA 追踪处理过程，给出带文件/段落依据的解释。
3. **核对出处**：要求展示 Evidence 记录（读了哪些文件、哪几段代码、每条结论基于什么），再用大白话讲一遍原理；找不到证据的结论让它重查。
4. **用自己的技术栈重写**：明确"基于理解的全新实现，不要照搬原代码"，要求列出文件名/位置/内容和运行预览步骤；验收三件事：跑不跑得起来、像不像原版、缺什么。
5. **PentAGI 安全体检**（要上线、有后端/表单/收用户数据时必做）：声明是自己有权测试的系统，只针对该目标，报告按严重程度解释并给修法。

卡住时的报错模板："我在【终端/客户端】做了【操作】，出现报错：【完整报错原文】，请先解释原因再给下一步，说明在哪个窗口做、成功后看到什么。"

## 小白常踩的坑

- 第一次就挑大目标 → 拆成小功能，先跑通最小的。
- 没看懂就让它写 → 先看懂再动手，否则不敢改。
- 把复刻当抄代码 → 看不懂改不动，且有版权/许可风险；看懂后重写才是你的。
- 环境没装齐就开工（Node 版本、Docker）。
- 报错只发一半 → 要发完整原文。
- 拿 PentAGI 测别人的网站 → 只能测自己有权测的系统。
- 漏洞想一次修完 → 先修严重/高危，中危攒着修，低危记下。

## 备注

- 本文是 10-12 REA 笔记的延伸：重点从"逆向"扩展到"复刻 + 安全验证"的完整流水线，并提供可直接粘贴给 Codex 的五段提示词（上文为要点摘录）。
- 法律提醒：逆向和复刻涉及软件许可协议与著作权，clean-room 重写也需注意界面近似等风险；渗透测试只能针对授权目标。上一篇 REA 笔记也提醒过警惕仿冒仓库，安装前核对官方地址。
