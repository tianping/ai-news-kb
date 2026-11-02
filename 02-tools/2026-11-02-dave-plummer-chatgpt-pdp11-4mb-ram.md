# 前微软工程师 Dave Plummer 用 ChatGPT 自制 PDP-11 4MB 内存板，跑起 UNIX 多用户模式

- **收藏日期**：2026-11-02
- **来源**：微信公众号「CSDN」（整理：屠敏）
- **原标题**：内存太贵，前微软工程师直接让AI帮忙造！给1970年古董电脑扩容4MB RAM，成功跑起UNIX
- **URL**：https://mp.weixin.qq.com/s/mRTrF1wK9ejRzAxSQzdSDw
- **原始出处**：Plummer 的 X 帖子 https://x.com/davepl1968/status/2115871903114260528 ；Tom's Hardware 报道 https://www.tomshardware.com/pc-components/ram/veteran-microsoft-engineer-makes-his-own-ram-from-scratch-using-ai-attempts-to-build-4mb-of-1970s-pdp-11-ram-from-scratch
- **分类**：02 工具与产品 / AI 辅助硬件开发

## 概述

Dave Plummer（Windows 任务管理器、Windows 弹球的开发者，已退休）嫌 PDP-11 原装内存太贵（约 1000 美元/MB，4MB 要 4000 美元），借助 ChatGPT 从零设计了一块 Q-Bus 内存板。他此前从没做过 FPGA，最后修好一个字节选择 bug，内存通过全部测试，并成功启动 UNIX 多用户模式。

## 关键内容

### 一、硬件设计（ChatGPT + EasyEDA）
- AI 协助完成原理图、PCB 布局布线和物料清单（BOM）。
- 目标机：DEC PDP-11（1970 年问世，UNIX 早期就在它上面发展），Q-Bus 总线，CPU 板 M8190。
- 板卡核心：Lattice MachXO2-2000HC FPGA 作内存控制器 + 4 颗 ISSI 1MB 存储芯片，另有 I/O 芯片、DIP 拨码开关、通信接口。
- 地址空间：22 位寻址，最高 4MB，顶部 256KB 保留给 I/O 和 ROM，内存板需遵守原有地址映射。
- 一次下单 5 块 PCB。

### 二、第一次烧 FPGA
- 买了 FT232H 转接板，用 openFPGALoader 烧录，失败几次后成功。
- 插入背板后系统识别出 4MB RAM 及 ROM 保留区。

### 三、踩坑：奇数地址访问失败
- PDP-11 是 16 位数据总线：读时 CPU 从 2 个字节中自选；写时用字节选择信号指明改哪一个字节。
- 最初 FPGA 代码没处理好字节选择，写一个字节会覆盖另一个 → 网友调侃「造了 4MB，结果只有 2MB」。
- 修改 FPGA 代码后通过全部测试，并启动 UNIX 多用户模式（对内存稳定性要求更高，验证更有说服力）。

### 四、另一种方案
- 有网友建议用树莓派接总线、软件模拟内存。Plummer 说上周已用 QBone 接口板（BeagleBone Black）做过软件方案；这次 FPGA 板是更接近传统硬件的实现。

## 启示
- AI 帮人跨过陌生领域（PCB、FPGA）的入门门槛，但实际调试——像字节选择这种总线细节——仍要靠人一点点排查、修改、验证。
- 「AI 辅助开发」正在从软件延伸到硬件。

## 备注
- 正文通过 fetch_wechat.py 抓取（js_content），本次未做逐条核实。文末大会广告未收录。
