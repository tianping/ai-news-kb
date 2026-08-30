# WorkBuddy 进阶二十：接入 Agnes 免费 API 创建生图和视频 Skill

> 来源：新智元 · 2026-08-30
> 链接：https://mp.weixin.qq.com/s/I8yazsLiLP_sCCuQf5WsUA

## 概要

Agnes AI（Sapiens AI 出品）无限期免费开放文本、图像、视频三大模态 API，无需绑卡。本文教程演示如何给 WorkBuddy 创建一个 Skill，实现一句话生图、一句话生视频（含中英文自动配音），全程零积分。

## Agnes 免费 API 能力

| 能力 | 模型 | 耗时 | 备注 |
|------|------|------|------|
| 文生图 | agnes-image-2.1-flash | ~5秒 | 中文提示词原生支持 |
| 图生图/编辑 | agnes-image-2.0-flash | ~5秒 | 风格转换、多图合成 |
| 文生视频 | agnes-video-v2.0 | ~2-3分钟 | 电影级质感 |
| 图生视频 | agnes-video-v2.0 | ~2-3分钟 | 支持图片转视频 |
| 视频音频 | 自动生成 | 随视频 | 实测支持中英文对话 |

- **Base URL**：`https://apihub.agnes-ai.com/v1`
- **认证**：Bearer Token，OpenAI 兼容协议
- **注册**：platform.agnes-ai.com，不绑卡
- **费用**：无限期免费

## 创建 Skill 流程

1. 注册 Agnes → platform.agnes-ai.com → 创建 API Key（关闭后不可再查看，务必保存）
2. 对 WorkBuddy 说：`我想使用 Agnes Image 2.0 模型生成图片和视频。请访问其 API 平台 https://agnes-ai.com/doc/overview 并将其打包为一个 Skill。`
3. WorkBuddy 自动：访问 API 文档 → 理解接口结构 → 生成 Skill 技能包 → 保存到 `~/.workbuddy/skills/agnes-image-video/`
4. 用 `.env` 文件隔离密钥（`AGNES_API_KEY` + `AGNES_BASE_URL`）
5. 中文化 SKILL.md：`把技能文档改成中文`
6. **重启 WorkBuddy**（右键托盘退出→重开）才能生效

技能结构：
```
~/.workbuddy/skills/agnes-image-video/
├── SKILL.md          ← 技能描述 + 触发词 + 操作流程
├── scripts/generate.py  ← 调用 Agnes API 的脚本
├── references/       ← API 参考文档
├── assets/           ← 资源文件
└── .env              ← API Key 配置（安全隔离）
```

## 使用方式

**文生图**：`用 Agnes 生成一张雪山下的红色跑车，电影感` → 5秒出图

**图生图**：`把这张图改成水彩画风格`（用 agnes-image-2.0-flash）

**文生视频/图生视频**：`把这张图生成视频，时长10秒`
- `num_frames` 必须满足 **8n+1** 格式（121/241/441）
- 视频时长速查：121帧@24fps≈5秒，241帧≈10秒，441帧≈18秒
- 视频生成是异步的：提交→返回 task_id→轮询→2-3分钟→返回视频 URL

**视频自带音频**：生成的视频含 AAC 格式音频，支持中英文对话，在 prompt 中描述台词模型会尝试按场景生成

## 踩坑记录

| 坑 | 解法 |
|----|------|
| 文生图报错 | 只传 model+prompt+size，不要传 extra_body（图生图才需要） |
| 视频 URL 字段名不一致 | 代码取 video_url 可能空，实际字段可能是 remixed_from_video_id，做兼容处理 |
| 技能创建后不生效 | 必须右键托盘完全退出 WorkBuddy 再重开 |
| 视频生成超时 | num_frames 必须 8n+1；网络慢时加长轮询超时，最长等 5 分钟 |

## 与 WorkBuddy 内置 ImageGen 对比

| 维度 | 内置 ImageGen | Agnes Skill |
|------|--------------|-------------|
| 积分 | 消耗积分 | 零积分 |
| 图片质量 | 高 | 高 |
| 中文 | 支持 | 原生支持 |
| 视频 | 无 | 支持 |
| 音频 | 无 | 视频自带中英文 |
| 门槛 | 零 | 需注册+创建 Skill |
| 适合 | 偶尔生图 | 高频生图+需要视频 |

## 关键启示

- Skill 系统的威力：一句话创建技能、自动生成脚本、安全隔离密钥、自然语言调用
- 配好一次，以后一句话出图、一句话生视频，零成本获得图像+视频能力
- 每次创建完技能让 AI 顺便整理开发文档放技能文件夹里，方便后续修改
