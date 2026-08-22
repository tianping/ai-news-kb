# FreeToken：单卡 5090 跑满血 DeepSeek V4 Flash

> 来源：机器之心（微信公众号）
> 编辑：泽南
> 原文链接：https://mp.weixin.qq.com/s/Hz-mm3Izi8M2uMZHL3wx5w
> 收录日期：2026-08-22

## 一、核心突破

来自 UC Berkeley、MIT 等机构的联合团队开源了名为 **FreeToken** 的端侧大模型推理系统。

- 让笔记本 RTX 4060 跑 Qwen3.6-35B（39.3 token/s）
- 让桌面 RTX 5090 跑 DeepSeek-V4-Flash 284B（单卡，满血模型）
- 笔记本 4060 的 39.3 token/s 已超过 Codex 生产 trace 中位 33 token/s

## 二、论文与开源信息

- **论文**：《FreeToken: Efficient Edge-Native MoE Serving with Bandwidth-Adaptive Execution》
- **arXiv**：https://arxiv.org/abs/2608.16157
- **GitHub**：https://github.com/FlashML-org/FreeToken
- **官网**：https://www.flashml.ai/
- 已提供 Windows/Linux 桌面 App（带 GUI）
- CLI 一行安装：`uv pip install "freetoken [accel]"`

## 三、团队与背景

- 并列一作：杨硕（Shuo Yang，伯克利 EECS 博士生）、范晓泽（Xiaoze Fan，UT Austin），两人本科均毕业于上海交通大学
- 作者包括 Ion Stoica、Matei Zaharia、Kurt Keutzer、韩松等

## 四、解决的问题

大模型开放权重解决了"谁能拿到模型"，但没解决"谁能跑得起模型"。Kimi-K3、GLM-5.2、DeepSeek-V4-Flash 能力已逼近闭源第一梯队，但跑它们仍默认要数据中心——能力差距在收窄，可及性差距依然巨大。

## 五、FreeToken 的四大技术

### 1. 全层双缓冲（Double-Buffered Prefill）
- 计算与数据搬运完全重叠
- GPU 计算第 l 层时，第 l+1 层 Expert 权重已通过 PCIe 后台预取
- 基本消除 I/O 等待气泡

### 2. 带宽自适应的混合调度（Bandwidth-Adaptive Execution）
- 运行时实时探测 PCIe 实际带宽与 CPU 瞬时算力
- 动态计算最优分流比率 q*
- 部分 Expert 命中 GPU LRU 缓存，未命中部分智能分流至 GPU/CPU

### 3. 面向 Agent 的智能状态复用（Agentic State Reuse）
- 在特殊 Token 边界设置轻量检查点
- 上下文编辑时仅从最近锚点恢复并增量 Prefill
- **长 Prompt TTFT 降低 42-58%**
- **多次工具调用后 TTFT 减少 65-80%**
- 适配 Claude Code、OpenClaw 等 Agent 工具的多轮交互场景

### 4. 弹性显存动态热扩缩容
- 后台游戏/3D渲染导致显存骤降 4-8GB 时自动平滑降级
- 0 停机开销，不会 CUDA OOM 崩溃
- 将更多 Expert 转交 CPU 计算

## 六、硬件需求与性能数据

| 项目 | 数据 |
|------|------|
| DeepSeek-V4-Flash 总参数 | 284B |
| 层数 | 43 层 |
| 每层路由专家 | 256 个选 6 个 |
| 单 token 激活 | 13B |
| RTX 5090 显存 | 32GB |
| 推荐内存 | 192GB（专家池约 140GB） |
| FP4 专家权重搬运 | 约 140GB |
| 测试 PCIe 规格 | 3.0 x8、4.0 x16、5.0 x16 |
| 测试 CPU | Intel i7、AMD Ryzen 9、Threadripper |

## 七、意义

让大模型走向普及的关键，不仅在于开源模型权重，更在于端侧调度软件系统的工程设计与算法重构。通过将 CPU、系统内存、PCIe 总线与 GPU 作为统一弹性计算平台，个人完全可以在本地跑起顶尖 MoE Agent。

**核心理念：显存不够，内存来凑。** 对 MoE 稀疏架构而言，通过"动态带宽感知 + 双缓冲数据流 + 状态复用"，可以用 DDR5 大内存 + PCIe + 消费级单卡获得可日常使用的体验。

## 标签

`FreeToken`, `DeepSeek-V4-Flash`, `MoE`, `端侧推理`, `本地部署`, `UC Berkeley`, `RTX 5090`, `开源推理系统`, `Agent`