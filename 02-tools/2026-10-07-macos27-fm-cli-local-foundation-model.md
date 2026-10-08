# macOS 27 自带本地 AI：终端敲 fm 就能聊（Apple Foundation Models 命令行）

- **日期**：2026-10-07
- **来源**：微信公众号「码上观星」
- **分类**：02 工具与产品
- **URL**：https://mp.weixin.qq.com/s/EzSvn6LLBvptNcy39QpPeA

## 概述

macOS 27 Golden Gate 预装了一个命令行工具 `fm`，可以直接调用驱动 Apple 智能的端侧模型 **Apple Foundation Models**（约 30 亿参数量级）。不用装 Ollama，不用下模型，也不用 API Key。以前开发者得写 Swift 才能调用。模型下好后可以离线跑，走默认 system 模型时数据不出本机。

**定位**：系统内置的文本小工具。适合摘要、改写、翻译短文本，从日志或邮件里按格式抽信息，嵌进 shell 脚本做自动化。**不适合**写复杂代码、解难题、长文档和多步推理，上下文窗口约 4K tokens 量级。苹果自己的开发者材料也暗示过，代码、数学、强逻辑推理不是这个模型的主场。

## 前提

- macOS 27、Apple 芯片（M1 及以后），开启 Apple 智能
- 首次下载模型需要联网，之后可离线。模型占十几 GB，有人看到接近 30 GB
- 国行或地区设置可能导致 Apple 智能不可用；`fm available` 一直报未就绪，多半是系统开关或地区限制

## 用法

```bash
fm                         # 入口
sudo fm license            # 首次：用管理员权限同意条款（一次性）
fm available               # 看到 "System model available" 即就绪；modelNotReady = 还在后台下载，接电源 + Wi‑Fi 等一会

fm chat                    # 交互聊天（退出命令以本机提示为准）

fm respond "用三句话解释什么是端侧模型"                 # 一问一答，最适合嵌脚本
cat notes.txt | fm respond "用中文总结要点，不超过 5 条"   # 管道喂文件
fm respond --no-stream "把下面这段改成更口语：……"         # 关流式，方便脚本解析

fm serve                   # 起本地 OpenAI Chat Completions 风格接口，可接 Open WebUI / OpenAI SDK；端口或 Unix socket 以 --help 为准
```

- **结构化输出**：先定义 schema，再让 `fm respond` 按 schema 输出 JSON，交给 jq 或脚本处理。具体子命令以 `fm --help` / `fm schema --help` 为准，beta 和正式版可能有出入。典型场景：从客服对话抽情绪、意图、是否需人工；从报错日志抽错误类型、可疑文件路径；从会议纪要抽待办、负责人、截止日期。
- **模型名**：`system` 是本机端侧模型，默认、免费、可离线。`pcc`（Private Cloud Compute，苹果私有云上的更大模型）在部分 beta 的 CLI 里出现过，有反馈说正式版已去掉，以本机帮助信息为准。脚本里最好显式指定本机模型，别和云端混用。

**一句话**：本地小模型最稳的用法是**按格式抽信息**，不是天马行空地聊天。

## 5 分钟最小实验

```bash
fm available
fm respond "用一句话介绍你自己"
echo "明天三点和客户开会，记得带合同" | fm respond "提取待办，中文短句"
```

## FAQ

- **找不到 fm 命令**：多半还没升到 macOS 27，或 PATH 有问题。
- **和 Ollama / MLX 比**：不是一个赛道。fm 胜在零安装、和系统一体、默认本地隐私；开源本地栈模型可选、能力上限更高。很多人两边都留着。
- **太占空间**：在系统设置里关掉 Apple 智能可以减少后续占用，但官方没有一键卸载模型的入口。

## 备注

- 子命令、端口、PCC 选项是否保留，原文多处注明以本机 `fm --help` 为准，没有核实。
- **和 RemoveMacAI 冲突**：本库 [RemoveMacAI](2026-10-07-removemacai-disable-apple-intelligence.md) 会注销 foundation models、释放空间，用了它 `fm` 大概率就不能用了。想留 fm，就别全关，或者先看 `removemacai features` 里有没有对应的保留标签（未核实）。
- 相关笔记：[macOS 27 Golden Gate 15 个隐藏功能](2026-10-07-macos-27-golden-gate-15-hidden-features.md)。
