# The Art of Command Line —— 一页纸讲透命令行的 GitHub 圣经

- **仓库**: [jlevy/the-art-of-command-line](https://github.com/jlevy/the-art-of-command-line)
- **Stars**: 162,157（2026-08-31 快照）
- **默认分支**: master；最近一次 push 2024-06-25
- **许可**: CC BY-SA 4.0
- **来源**: [开源软件社公众号](https://mp.weixin.qq.com/s/pjHal2Q_DqOqLPZERbTIrg)

## 它是什么

不是教程，而是作者们在 Linux 工作中沉淀的**命令行笔记合集**，官方口号："Master the command line, in one page"。三条自我要求：

- **广度**：重要的都覆盖到
- **具体**：给出最常见场景的具体例子
- **简短**：不写非必要内容，每条技巧必须"在某种情况下必不可少，或明显节省时间"

聚焦**交互式 Bash**，正文面向 Linux，另设 macOS / Windows 专属章节。起源于 Quora 上的 Unix 技巧回答，后搬到 GitHub 由社区共同维护，已被翻译成十几种语言（含简中/繁中）。作者 jlevy 正在公开招募新合著者，计划扩展得更广更深。

## 目录结构一览

| 章节 | 亮点内容 |
|------|---------|
| Basics | `man bash`、Vim/nano/Emacs、重定向/管道/通配符/引号、作业管理（`&`、`ctrl-z`、`jobs`、`fg/bg`）、ssh、git、包管理器（apt/yum/dnf/pacman/pip） |
| Everyday use | Tab 补全、`ctrl-r` 历史搜索、readline 快捷键（`ctrl-w`/`ctrl-u`/`alt-b`/`alt-f`）、`set -o vi`、`!$`/`!!`、`cd -`、xargs/parallel、pstree/pgrep/pkill、nohup/disown、`~/.bashrc` |
| 脚本安全气囊 | `set -euo pipefail` + trap 严格模式 |
| Processing files & data | find/locate、ack/ag/**rg**、pandoc、xmlstarlet、**jq**、csvkit、sort/uniq 集合运算、cut/paste/join、awk/sed、`perl -pi`、repren 批量改名、rsync、shuf、diff/patch、iconv/uconv、zcat/zgrep |
| System debugging | curl/wget/httpie、top/htop、iostat/iotop、netstat/ss、dstat/glances、mtr、ncdu、iftop/nethogs、ab/siege、wireshark/tshark/ngrep、**strace/ltrace**、gdb、`/proc`、sar、dmesg、`kill -3`（Java 栈） |
| One-liners | sort/uniq 集合交并差、awk 一行求和、`watch` 持续监控、`taocl` 随机抽题函数 |
| Obscure but useful | expr、m4、yes、cal、env、look、fmt、pr、fold、column、nl、seq、bc、factor、gpg、nc、socat、dd、file、tree、stat、timeout、lockfile、tac、comm、strings、tr、`split/csplit`、sponge、units、xz、ldd、nm、cssh 等 50+ 冷门命令 |
| macOS only | brew/port、`pbcopy/pbpaste`、`mdfind/mdls`、BSD vs GNU 工具差异 |
| Windows only | Cygwin、WSL、MinGW/MSYS、wmic |

## 值得记住的冷知识

- `man ascii` 直接看 ASCII 表
- `ctrl-v` + Tab 输入真实制表符
- `alt-#` 把写了一半的命令注释掉留待后用
- `<(some command)` 进程替换：可直接 `diff <(ssh host cat file) localfile`
- `ldd` **不要**对不可信文件运行（可执行任意代码）
- 命令行参数有约 128K 上限，报 `Argument list too long` 时用 xargs
- 文件名可能含空格：注意引号与 `-print0` / `-0` 约定
- "你能用 Bash 做到某件事，不代表你就应该做"

## 自带的复习工具：taocl

文档内置 `taocl` 函数——抓取 README 本身随机抽一条技巧打印，把文档当题库用，适合每日一题。

## 生态与延伸

- [awesome-shell](https://github.com/alebcay/awesome-shell)
- [ShellCheck](https://www.shellcheck.net/) — Bash 静态检查
- *Data Science at the Command Line*（书）

## 客观评价

- **不是系统教程**：按引用包含内容，默认你自己去查细节，纯小白读起来可能跳跃
- 重心在交互式 Bash 与 Linux；深度脚本工程、特定发行版细节非其主战场
- Star 多 ≠ 适合你：不常碰终端的话先读 Basics 即可，不必一次吞下 50+ 冷门命令
- 作者自认"本可以更广、更深"，所以仍在招募合著者

## 适合谁

- **新手**：从 Basics + `man bash` + nano/Vim 起步
- **老手**：直奔 Obscure but useful 和 One-liners 查漏补缺
- **运维/SRE**：System debugging 整章是排障弹药库
- **做培训的人**：taocl 随机抽题 + 多语言版本，直接当教材
