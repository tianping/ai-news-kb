# 商汤 Token Plan 免费额度实测：每5小时1500次调用，四个模型任选

> 来源：[免费白嫖商汤 Token Plan：5 小时 1500 次调用，四个模型任选](https://mp.weixin.qq.com/s/6W1iHP-mnm1FmugLOyht-A) · 2026-08-21

## 概述

商汤 Sensenova Token Plan 公测期间免费，4 个模型可选，配额每 5 小时刷新。手机号注册无需绑卡，兼容 OpenAI 接口。

## 免费额度（每 5 小时）

| 模型名称 | Model ID | 调用次数 | 描述 | 推荐场景 |
|----------|----------|----------|------|---------|
| SenseNova 6.8 Flash-Lite | `sensenova-6.8-flash-lite` | 1500 次 | 原生多模态，读文档/图片/表格，OCR、图表解读 | 日常主力：对话、办公、截图照片直接丢 |
| SenseNova U1 Fast | `sensenova-u1-fast` | 1500 次 | 多模态理解生成，图文内容是强项 | 信息图、图文生成、办公交付物备用 |
| DeepSeek V4 Flash | `deepseek-v4-flash` | 500 次 | 轻量快跑，工具调用，256K 长上下文 | 纯文本快问快答、长文档阅读 |
| GLM-5.2 | `glm-5.2` | 500 次 | 通用问答、复杂推理、项目级工程 | 纯文本快问快答、长文档阅读 |

> ※ 免费公测期额度随时可能调整，以控制台为准。

## 实测体验

- 注册方式：手机号即可，不用充值、不用绑卡
- Base URL：`token.sensenova.cn/v1`（以官方文档为准）
- 兼容 OpenAI 接口格式
- GLM-5.2 实测出现频繁限流报错 `Quota exceeded: rpm exhausted`，切回 SenseNova 6.8 Flash-Lite 后无此问题
- 6.8 Flash-Lite 已支持多模态（截图、照片可直接丢给模型）

## 接入方法

1. 注册 https://platform.sensenova.cn/console，创建 API Key
2. Base URL 填 `token.sensenova.cn/v1`
3. 按上表添加模型（注意区分模型名称和 Model ID）

## 与上一篇的区别

上一篇（WorkBuddy 接入教程）侧重工具配置步骤，本篇侧重**额度实测 + 各模型体验对比**，补充了 GLM-5.2 限流问题、6.8 vs 6.7 版本号修正等实用信息。
