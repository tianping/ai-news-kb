# 开源项目解锁 GeForce 显卡 Tensor Core 限速：RTX 4090 特定运算吞吐翻倍

- **日期**：2026-10-26
- **来源**：微信公众号「机器之心」
- **原标题**：英伟达GPU隐藏算力被开源解锁！不改官方驱动，RTX 4090吞吐翻倍
- **分类**：04-papers（技术突破 / GPU 硬件）
- **URL**：https://mp.weixin.qq.com/s/UUR-hbnGG6b8TXOdqNDnqQ

## 概述

GitHub 上一批开发者发现：部分 GeForce 显卡的 Tensor Core 在执行 **FP16/BF16 输入、FP32 累加** 的混合精度运算时，被限制了指令发射速率（数据中心卡没有这个限制）。在 GPU 启动阶段改写控制速率的寄存器后，这条运算路径的吞吐率可以恢复约 2 倍。不换硬件、不刷 BIOS、不改官方驱动、不提高功耗上限。RTX 20/30/40 三代都已有开源项目，Windows 11 上也有实验成功。

## 原理

- GPU 内部有控制不同指令执行速率的寄存器（如 **SS0、SS1**），受权限保护，普通驱动不能改
- Zagitta 借鉴此前 CMP 矿卡解锁项目的 **V67** 技术：利用 GPU **安全启动固件中的漏洞**，用返回导向编程（ROP）修改权限级别掩码（PLM），取得这些寄存器的写权限
- 把 SS0 设为 `0x88888888`、SS1 设为 `0x8`，对应运算路径切到更高发射速率
- 做成 Linux 内核模块：在 NVIDIA 官方驱动接管前运行 → 改寄存器 → PCIe 功能级重置（FLR）使配置生效 → 原版驱动正常加载；驱动初始化后检查寄存器，配置仍有效
- Windows / 部分 Turing 方案改用 **UEFI 程序**，在操作系统启动前完成修改

## 各代测试结果

### RTX 40（Ada）— gpu-unlock-ada（JustAResearcher，2 天 2.7k star）

RTX 4090（AD102，128 SM，450 W 上限，NVIDIA 615.78.08 驱动）：

| 运算 | 解锁前 | 解锁后 |
|---|---|---|
| FP16 入 / FP32 累加（指令吞吐） | 165.6 TFLOPS | 331.6 TFLOPS |
| BF16 入 / FP32 累加（指令吞吐） | 165.5 TFLOPS | 330.6 TFLOPS |
| FP16 入 / FP16 累加 | 325.7 | 331.6（本来就不受限） |
| INT8 | ~661 TOPS | 不变 |
| FP32 标量 | ~79.1 TFLOPS | 不变 |

- 另一位 4090 用户独立复测：FP16/FP32 160.4 → 320.4，BF16/FP32 159.7 → 320.2 TFLOPS
- **实际 cuBLAS 矩阵乘法增幅小得多**：FP16 提升约 12%–34%（4096² 从 153.2 → 206.0 TFLOPS）；BF16 在 1024²/8192² 提升约 20%/16%，4096² 基本不变

### RTX 30（Ampere）— gpu-unlock（Zagitta）

| 显卡 | 结果 |
|---|---|
| RTX 3090 Ti（450 W） | BF16 87.5 → 157.4 TFLOPS（+约 80%）；TF32 +94% |
| RTX 3080 10GB | BF16 65.4 → 113.4（+73%）；FP16/FP32 64.88 → 110.14（+70%） |
| RTX 3070 / 3060 | 约 +81% / +76% |

- RTX 3080 120 秒持续测试：完成运算次数 6940 → 11860（+71%），功耗都约 318 W，峰值温度 78/79 ℃；FP16/BF16/TF32 输出解锁前后完全一致
- **真实 AI 任务**：RTX 3090 用 vLLM 跑 27B BF16 模型，**预填充（prefill）快约 33%，首 token 延迟降约 24%**；两张显存扩容版 3080 预填充快 33%–36%
- **逐 token 生成速度基本不变**：生成阶段受显存带宽限制，不吃 Tensor Core 算力

### RTX 20（Turing）— 8 年前的 2080 Ti

- **turing-unlock（nlzy，Linux 内核模块）**：Dell RTX 2080 Ti 22GB 改装卡、250 W，cuBLAS FP16/FP32
  - 2048³：55.7 → 91.4 TFLOPS（+64%）；1024：+36%；4096：+52%；8192：+48%
  - 官方白皮书标称峰值 56.9 TFLOPS → 解锁前达 97.9%，解锁后达 160.6%，说明原来确实被限速
- **Turing-unlock（haohervchb，UEFI 方案）**：ASUS RTX 2080 Ti（TU102，68 SM，11GB）
  - 底层 MMA 指令：46.94 → 93.66 TFLOPS（约 511.7 → 1022.4 FLOP/SM/clock）
  - cuBLAS 8192² GEMM：48.15 → 85.17 TFLOPS（+77%）
- 验证范围有限：只证明部分 TU102 卡可行，其他 20 系型号及不同驱动/固件版本兼容性未知

### Windows 11 — AD103_win_unlocker（TOTORONG，Hugging Face）

- 32GB 显存扩容版 RTX 4080（AD103），UEFI 程序在 Windows 启动前修改
- 4096 矩阵乘法：FP16/FP32 91.54 → 182.80，BF16/FP32 91.91 → 184.49 TFLOPS
- 仅限特定改装卡，不能视为普通 4080 或整个 40 系通用

## 要点总结

- 解锁的只是**特定 Tensor Core 路径**（FP16/BF16 入、FP32 累加），不是整卡算力翻倍
- 底层指令吞吐 ~2 倍，实际矩阵乘法 +12%～+80% 不等，大模型推理主要加速**预填充**阶段
- 对本地跑大模型、做训练的消费级显卡用户有实际价值

## 备注（风险）

- 方案依赖 GPU 安全启动固件的漏洞，NVIDIA 很可能通过新驱动/固件封堵；可能违反驱动许可条款、影响保修
- 社区实验性项目，长期稳定性未经充分验证；文章本身未讨论这些风险，为本笔记补充
- 项目地址：
  - https://github.com/JustAResearcher/gpu-unlock-ada
  - https://github.com/Zagitta/gpu-unlock
  - https://github.com/nlzy/turing-unlock
  - https://github.com/haohervchb/Turing-unlock
  - https://huggingface.co/TOTORONG/AD103_win_unlocker
