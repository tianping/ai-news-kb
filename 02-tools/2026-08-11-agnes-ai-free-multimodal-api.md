# Agnes AI：免费全模态API（文本+图片+视频）

> 来源：微信公众号文章 · 2026-08-11 · [原文链接](https://mp.weixin.qq.com/s/SDLNro0joJG30ZVgK5kGsw)

## 概述

Agnes AI 把三款全模态模型 API 开放免费使用（无限期，非试用），支持文本、图片、视频。OpenAI 兼容接口，迁移成本极低。

- **GitHub**：https://github.com/AgnesAI-Labs/Agnes-AI
- **平台**：https://platform.agnes-ai.com
- **文档**：https://agnes-ai.com/doc/overview

## 使用数据

单周总 Token **4.11万亿**，OpenRouter 上仅次于 DeepSeek V4 Flash：
- 文本模型贡献 2.67万亿
- 图片和视频 1.44万亿
- 一周生成图片 567万+张，视频 237万+秒

## 三款模型

| 模型 | 能力 | 规格 |
|------|------|------|
| `agnes-2.0-flash` | 文本+视觉语言模型，支持推理/编程/工具调用/流式输出/图片理解 | 上下文 256K |
| `agnes-image-2.1-flash` | 图片生成和编辑（文生图/图生图） | 最高 4096×4096 4K |
| `agnes-video-v2.0` | 视频生成（文生视频/图生视频/多图视频/关键帧动画） | 异步任务模式 |

## 上手步骤

### 1. 获取 API Key
- 注册 https://platform.agnes-ai.com（邮箱验证，不绑信用卡，不实名）
- 创建 API 密钥，只显示一次

### 2. Python 调用

```python
from openai import OpenAI

client = OpenAI(
    api_key="你的_API_Key",
    base_url="https://apihub.agnes-ai.com/v1"
)

response = client.chat.completions.create(
    model="agnes-2.0-flash",
    messages=[{"role": "user", "content": "写一段 Agnes AI 的简介"}],
    stream=True,
)

for chunk in response:
    delta = chunk.choices[0].delta.content
    if delta:
        print(delta, end="")
```

### 3. 接入 Claude Code
通过 CC-Switch 配置：
- 供应商名称：Agnes AI
- 请求地址：`https://apihub.agnes-ai.com/v1`
- API格式：OpenAI Chat Completions
- 模型映射：Sonnet/Opus/Haiku 均填 `agnes-2.0-flash`
- 勾选"声明支持 1M"
- 配置 JSON 加：
```json
{
  "allowed_openai_params": ["thinking", "context_management"],
  "litellm_settings": {"drop_params": true}
}
```

### 4. 装 Skill 解锁全模态
社区 Skill：`agnes-ai-generation-skill`（github.com/Yacey/agnes-ai-generation-skill）

在 Claude Code 里发：
```
帮我安装这个 Skill 在当前项目。https://github.com/Yacey/agnes-ai-generation-skill
```

装完后可直接对话生图/生视频，自动中文提示词翻译英文。

## 额度与限制

| 项目 | 免费用户 | Token Plan |
|------|----------|------------|
| 文本模型 | 20 RPM | 1000 RPM |
| 视频模型 | 1 RPM | 500秒/天 |

## 社区生态
- ComfyUI 自定义节点插件
- 全栈图片视频生成平台
- 新手排查 Helper Skill
- GitHub Issues/Projects 看板公开

## 适合谁用

- 想找免费全模态 AI API 的开发者
- 想给 Claude Code / Cursor / Codex 换免费底层模型的人
- 做个人项目或 Demo 的开发者和小团队
