# SenseNova U1.5 Lite 正式版发布：8B 原生统一多模态，支持超长指令与原生4K

> 来源：[SenseNova U1.5 Lite正式版发布！支持超长指令，解锁原生4K真实视觉创作流](https://mp.weixin.qq.com/s/JVFdroJihsHTBDllS0DFCg) · 商汤科技 · 2026-08-21

## 概述

商汤 SenseNova U1.5 Lite 正式版开源，8B 参数轻量模型，基于 NEO-unify 统一架构，主打**超长指令遵循**、**原生4K输出**和**高可控视觉创作**，支持单卡部署。

## 核心能力

### 1. 3-4k 字符超长复杂指令遵循
- 打破大多数模型在指令超 1k 字符时崩溃/遗漏的瓶颈
- 原生支持 3-4k 上下文长度
- 可同时处理主体、数量、空间关系、文字、布局、风格等多重约束

### 2. 高质量视觉生成
- 改善整体构图、色彩、材质、光影、真实感和局部细节
- 减少"局部正确但整体完成度不足"

### 3. 原生图像编辑
- 增强主体身份、空间结构、版式关系保持
- 局部修改、元素替换、文字精修、多参考图编辑

### 4. 文字与复杂布局
- 中英文文字、海报、信息图、品牌视觉、多文本排版
- 从"生成内容"走向"组织完整视觉表达"

### 5. 精细视觉控制
- 支持 Bounding Box、Visual Marker
- 单图/多图参考

### 6. 原生4K高分辨率输出
- 高分辨率下兼顾整体构图与极精细肌理、微小文字与光影折射

## 架构亮点

### NEO-unify 统一架构 + MOPD 蒸馏
- **训练阶段**：针对文字、美学、编辑独立训练专家模型（Expert）
- **交付阶段**：多专家在线策略蒸馏（MOPD）将多专家能力无损融合至单体 8B 模型
- **双向反哺**：视觉语义理解与空间建模反哺生成

### 极致性价比
- 8B 参数量，单卡流畅运行
- 无需 Router 频繁切换
- 单次 Pass 兼顾语义理解与像素生成

### RL 后训练强化
- 指令遵循（Instruction Compliance）
- 视觉偏好（Visual Preference）
- 编辑保持（Editing Preservation）

## 开源地址

| 平台 | 链接 |
|------|------|
| GitHub | https://github.com/OpenSenseNova/SenseNova-U1 |
| Hugging Face | https://huggingface.co/collections/sensenova/sensenova-u15 |
| 魔搭社区 | https://modelscope.cn/models/SenseNova/SenseNova-U1.5-8B-MoT |
| 在线体验 | https://unify.light-ai.top/ |

## 与同类模型对比

- 8B 参数下指令遵循与图像编辑保持度超越同量级模型
- 文字渲染与复杂布局媲美超大规模商业模型
- 支持多轮迭代修改不跑偏
