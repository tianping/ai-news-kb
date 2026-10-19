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

## 补充（2026-10-19）：「AI公园」实测 10 个用法

- **来源**：微信公众号「AI公园」《苹果在 macOS 27 藏了个离线 AI，不联网不用钱》，译自 Medium @macoclock
- **URL**：https://mp.weixin.qq.com/s/wqUvTCx-WufMPmKcHOo5Tg
- **英文原文**：https://medium.com/macoclock/macos-27-has-a-hidden-llm-inside-10-amazing-things-you-can-do-with-it-44a28d8cc042
- **测试机**：MacBook Air M1 8GB（入门款也能跑）。`fm` 位于 `/usr/bin/fm`。

**子命令全貌**（`fm --help`）：`respond` 单次问答、`chat` 持续对话、`token-count` 数 token、`schema` 生成 JSON 结构、`serve` 本地 API、`quota-usage` 查云端额度。底层模型从 macOS 26 就已存在，但那时只能通过快捷指令的「使用模型」动作调用；27 新增的是可以接管道、写脚本的命令行入口。

**比上文多出的细节**：

1. **一问一答**：`fm respond "Swift 是什么"`。模型作为系统服务懒加载，冷启动后一般一两秒出结果。
2. **管道**：`cat notes.txt | fm respond "把重点列成三条"`、`pbpaste | fm respond "改写得更正式"`。作者实测它会编造开场白，适合做格式化和粗加工，结论要自己核对。
3. **会话持久化**：`fm chat` 的会话自动存到 `~/.fm/sessions/`（一小份 JSON，可随时删），退出时会打出恢复命令。
4. **读图**：`fm respond --image ~/Desktop/menu.png "图里有什么"`。截图描述大致准确，复杂表格识别不了，8GB M1 能跑。
5. **结构化 JSON**：`fm schema object --name Person --string name --int age` 生成结构文件，再 `fm respond --schema schema.json "提取这段话里的人名和年龄"`；支持嵌套对象和数组。
6. **打标签模式**：`fm respond --use-case content-tagging "…"`，返回关键词，适合书签、工单、照片说明。
7. **上下文 4096 token 是硬上限**（指令 + 输入 + 输出合计，约三千英文词），超了直接报错。可以先用 `fm token-count "…"` 数；PCC 约 32K。
8. **常用参数**：`--instructions`（人设/规则）、`--greedy`（降随机性）、`--temperature`、`--model pcc`。写脚本最稳的组合是 `--instructions` + `--greedy`。
9. **`fm serve`**：OpenAI 兼容接口（`/v1/chat/completions`），**默认端口 1976**（苹果成立年份），默认只监听 127.0.0.1，加 `--host 0.0.0.0` 开放到局域网，也支持 `--socket` 用 Unix socket；模型名填 `system` 或 `pcc`，不需要 API key。iPhone 可以用快捷指令 POST 过去：`model: system`、`stream: false`，结果取 `choices[0].message.content`。⚠️ **没有密码**，只在家里 Wi‑Fi 或私有组网里开，别暴露到公网。机器越强（M3/M4），回答质量越好。
10. **`--model pcc`**：请求发到 Private Cloud Compute，无状态、用完即销毁，苹果公开了服务器源码与加密证明供第三方审计；有**每日额度**，绑定 Apple 账户，用 `quota-usage` 查，iCloud+ 可提高上限。（这次实测 PCC 仍可用，和上文「正式版可能已去掉」的说法不一致，以本机为准。）

**能力边界**：
- 读不到备忘录、短信、日历、文件，也不知道当前时间；但它会自信地声称自己可以访问，属于幻觉。
- 知识截止约 2024 年 9 月（认为 iPhone 15 是最新款），不能联网。
- 苹果称它可与 3–4B 级别模型相当，独立评测大致在 Gemini Nano / 小型 Llama 档位；PCC 高一个量级，但追不上云端旗舰。
- 不替代 70B 本地模型、自定义权重或本地知识库方案。

**作者最常用的三件事**：乱笔记整理成清单、从日志提取字段、长邮件摘要。

## 备注

- 子命令、端口、PCC 选项是否保留，原文多处注明以本机 `fm --help` 为准，没有核实。
- **和 RemoveMacAI 冲突**：本库 [RemoveMacAI](2026-10-07-removemacai-disable-apple-intelligence.md) 会注销 foundation models、释放空间，用了它 `fm` 大概率就不能用了。想留 fm，就别全关，或者先看 `removemacai features` 里有没有对应的保留标签（未核实）。
- 相关笔记：[macOS 27 Golden Gate 15 个隐藏功能](2026-10-07-macos-27-golden-gate-15-hidden-features.md)。
