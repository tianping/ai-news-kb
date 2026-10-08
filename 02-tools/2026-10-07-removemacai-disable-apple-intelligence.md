# RemoveMacAI：一条命令停用 Apple Intelligence，腾出几十 GB 存储（可逆）

- **日期**：2026-10-07
- **来源**：微信公众号「极客精研社」
- **原标题**：拯救小容量 Mac！彻底停用 Apple Intelligence，一次性腾出几十 G 存储
- **分类**：02 工具与产品
- **URL**：https://mp.weixin.qq.com/s/HdXDBM6ZJox1jasrNrOspw
- **项目**：https://github.com/omlahore/RemoveMacAI（开源，Swift / Objective-C / Shell）

## 概述

macOS 27 没有「彻底关闭 Apple Intelligence」的总开关。在设置里逐个关掉 Siri、写作工具、通知摘要，只是关掉了功能入口；系统之前自动下载的基础模型、图像生成模型、代码补全模型仍然占着几十 GB 本地空间。RemoveMacAI 用一条命令停用全部 AI 功能并释放模型空间，**不需要关 SIP，也完全可逆**。对 256G、512G 的入门款 Mac 最有用。

## 为什么不能直接 rm -rf

- 系统卷受 SSV（已签名系统卷）和 SIP 保护。强行关 SIP 去删，会破坏完整性验证，影响 OTA 升级，甚至搞坏系统组件。
- 后台的 Asset 资产服务只要发现功能激活或策略有依赖，就会在夜间充电时把模型静默**重新下载**回来。

## 实现原理（都在苹果规则之内）

1. **配置描述文件**（Configuration Profile）：用苹果官方的 MDM 限制键，把 Apple Intelligence 相关功能锁定为停用。
2. **调用系统 Asset Service 注销资源**：释放基础模型、Genmoji / Image Playground、照片 Clean Up、Xcode 代码预测等模型，走合法请求而不是物理删除，不碰 `/System`。
3. **回环端口重定向**：把模型下载请求指向本机一个关闭的端口，阻止后台重新下载，也不会发出外部网络请求。

## 影响范围

**会被停用、释放空间的**：Siri（语音唤醒、菜单栏图标）、写作工具与内联文本预测、邮件/短信/Safari/备忘录/通知的智能摘要、邮件智能回复、Genmoji 与 Image Playground、照片 Clean Up 与空间照片转换、系统集成的 ChatGPT 扩展、Xcode 预测性代码补全。

**不受影响的**：
- **语音听写**（Dictation）：独立的语音识别模块，模型不会被删。
- **Spotlight 基础搜索**：活动监视器里仍能看到一个 Siri 进程，属正常现象。macOS 27 把 Spotlight 前端挂在 Siri 进程容器下，这个常驻进程受 SIP 保护，只负责基础检索，模型已卸载。

## 用法

```bash
brew install omlahore/tap/removemacai
# 或：curl -fsSL https://raw.githubusercontent.com/omlahore/RemoveMacAI/main/install.sh | bash

removemacai                     # 列出各 AI 模块状态与磁盘占用，安装描述文件前会二次确认
removemacai features            # 查看可保留的功能标签
removemacai off --keep photos-cleanup,xcode-completion   # 停用其余，保留消除和 Xcode 补全
removemacai status              # 查看当前占用
removemacai revert              # 撤销：移除描述文件和重定向规则
```

撤销后，在系统设置里重新勾选某项 AI 功能，macOS 会按需从官方服务器重新下载模型，对后续 OTA 升级没有影响。

## 备注

- 适合入门容量的 Mac，以及平时主要用云端模型或第三方本地推理工具的人。1TB 以上、经常用邮件摘要或照片消除的人没必要折腾。
- 「几十 GB」「上千 Star」「不带追踪代码」都是原文说法，没有逐一核实。`curl | bash` 安装前建议先看一下 install.sh。
- 相关笔记：[macOS 27 Golden Gate 15 个隐藏功能](2026-10-07-macos-27-golden-gate-15-hidden-features.md)。想用 Spotlight 问答、邮件智能回复、视觉智能这类 AI 功能，就别全关，用 `--keep` 保留需要的项。
- 相关笔记：[macOS 27 自带 fm 命令行本地模型](2026-10-07-macos27-fm-cli-local-foundation-model.md)。本工具注销 foundation models 后，fm 大概率不可用。
