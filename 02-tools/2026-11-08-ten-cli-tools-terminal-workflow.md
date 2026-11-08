# 10 个改变终端工作流的 CLI 工具：fzf、btop、tmux、lazygit、gh、entr、just、Taskwarrior、tldr、pet（程序员小黑）

- **日期**：2026-11-08（收藏；原文约 2026-10-22）
- **来源**：微信公众号「程序员小黑」
- **原标题**：10 个真正改变程序员终端工作流的 CLI 工具
- **分类**：02-tools（终端 / 开发者工具）
- **URL**：https://mp.weixin.qq.com/s/LdGE8cy742q8URC5peL1gw

## 概述

文章的观点：终端最费时间的不是命令执行，而是找命令、切窗口、找文件、看状态、重复执行同一套操作。下面 10 个工具分别解决其中一类问题，不必一次全装，从自己最痛的点开始。

## 工具清单

| # | 工具 | 解决什么 | 项目 | 同类/替代 |
|---|------|---------|------|----------|
| 1 | **fzf** | 模糊搜索历史命令、文件、Git 分支（"执行过但想不起怎么写"） | https://github.com/junegunn/fzf | — |
| 2 | **btop** | 终端里看 CPU/内存/磁盘/网络/进程 | https://github.com/aristocratos/btop | bpytop 是较早的前身 |
| 3 | **tmux** | 分窗/分面板，会话持久，SSH 断了进程照跑 | https://github.com/tmux/tmux | Zellij（更现代） |
| 4 | **lazygit** | 终端 Git 界面：分支、提交、暂存区、diff 一屏看完 | https://github.com/jesseduffield/lazygit | — |
| 5 | **gh** | GitHub 官方 CLI：仓库、Issue、PR、Actions 运行状态 | https://github.com/cli/cli | — |
| 6 | **entr** | 文件变了自动跑命令（改代码自动跑测试） | https://github.com/eradman/entr | watchexec（规则更丰富） |
| 7 | **just** | 用一个 justfile 统一项目常用命令：`just test` / `just build` / `just deploy` | https://github.com/casey/just | make |
| 8 | **Taskwarrior** | 命令行待办：优先级、截止时间 | https://github.com/GothenburgBitFactory/taskwarrior | 待办少的话纯文本就够 |
| 9 | **tldr** | 常用命令的简明示例（tar 怎么解压、curl 怎么 POST） | https://github.com/tldr-pages/tldr | 深入细节仍看 man |
| 10 | **pet** | 保存并搜索复杂命令片段（Docker/K8s/SSH/Git 高级用法） | https://github.com/knqyf263/pet | navi（交互式速查） |

## 组合用法

- **just + entr**：just 统一测试命令，entr 在文件变化时自动触发 `just test`。
- **tmux + fzf**：tmux 管多个长期任务，fzf 搜历史命令。
- 按痛点挑：常忘命令 → fzf、pet；Git 分支多 → lazygit；常 SSH 管服务器 → tmux、btop。

## 作者观点

AI Coding 普及后 CLI 并没有过时，代码执行、Git、测试构建、日志排查、服务器管理仍在终端。与其追求装了多少工具，不如先找出每天最浪费时间的三个动作，再用工具简化。

## 结合本机（McCleary）

- 2026-11-08 查了一下：`tmux`、`gh` 已在 PATH 里；fzf、btop、lazygit、entr、just、task、tldr、pet 都没装。
- fzf、lazygit、just、pet 都有预编译的单文件二进制，放进 `~/bin` 就能用，不需要 root。btop 也有静态构建版。entr 和 Taskwarrior 需要自己编译。
- 在 HPC 登录节点上，btop 只能看到当前节点，看不到作业所在的计算节点；查作业资源还是用 Slurm 自带的 `squeue`/`seff`/`sstat`。

## 相关笔记

- [跑 Agent 的终端复用器之争：Herdr vs Rex](2026-11-05-herdr-rex-agent-terminal-multiplexers.md) — tmux 之外面向 AI Agent 的终端复用器
- [DoOps：用「公网 Gateway + 内网 Agent」替代 SSH](2026-11-08-doops-ssh-alternative-gateway-agent.md)

## 备注

入门向清单，都是知名老牌工具，文章没有给安装命令或配置示例；具体用法以各项目 README 为准。
