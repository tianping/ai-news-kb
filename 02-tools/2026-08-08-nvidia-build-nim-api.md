# NVIDIA Build：100+ AI 模型免费 API 平台

> 来源：[NVIDIA Build 官网](https://build.nvidia.com) · [yangmao.ai - NIM 免费 API 教程](https://yangmao.ai/zh/blog/nvidia-nim-free-api-guide/) · [AIEII - NVIDIA免费开放80+模型API](https://aieii.com/posts/2026-05-18-nvidia-free-50-model-api/) · 2026-08-08

## 这是什么

NVIDIA 在 build.nvidia.com 上提供了一个免费 AI 推理 API 平台，基于 NIM（NVIDIA Inference Microservices）技术。100+ 个顶级模型，不要钱，不要信用卡，OpenAI 兼容格式，中国大陆可直连。

- **官网**：https://build.nvidia.com
- **模型列表**：https://build.nvidia.com/models
- **API 端点**：https://integrate.api.nvidia.com/v1

## 为什么值得关注

| 优势 | 说明 |
|------|------|
| 100+ 模型 | 覆盖对话、编程、推理、多模态、视觉、语音 |
| 完全免费 | 无额度限制（已取消原来的 1000/5000 次限制） |
| 免信用卡 | 注册即用，不绑卡 |
| OpenAI 兼容 | 改一行 base_url 就能接入现有代码 |
| 中国大陆直连 | 不需要代理 |
| 40 RPM | 每分钟 40 次，可申请提升到 200 |

## 免费模型精选（截至2026年8月）

### 顶级对话模型
| 模型 | 模型ID | 特点 |
|------|--------|------|
| MiniMax M2.7 | minimaxai/minimax-m2.7 | 230B参数，编程/推理/办公全能 |
| Kimi K2.5 | moonshotai/kimi-k2.5 | MoE架构，100万上下文，中文顶级 |
| GLM-5 | z-ai/glm-5 | 智谱最新旗舰 |
| DeepSeek V3.2 | deepseek-ai/deepseek-v3.2 | 671B MoE，编程之王 |
| DeepSeek R1 | deepseek-ai/deepseek-r1 | 671B MoE，推理之王 |
| Qwen 3.5 | qwen/qwen3.5 | 阿里通义千问 |

### 推理 & 编程模型
| 模型 | 模型ID | 特点 |
|------|--------|------|
| Nemotron-3-Super-120B | nvidia/nemotron-3-super-120b-a12b | NVIDIA自研，推理强 |
| Llama 4 | meta/llama-4 | Meta最新开源 |
| Gemma 4 31B-IT | google/gemma-4-31b-it | Google最新，Agentic能力强 |
| Step 3.5 Flash | stepfun-ai/step-3.5-flash | 阶跃星辰，速度极快 |

> 完整列表访问 https://build.nvidia.com/models

## 注册教程（3分钟）

1. 访问 https://build.nvidia.com，右上角 Login / Sign Up，邮箱注册
2. 登录后访问 https://build.nvidia.com/settings/api-keys，点击 Generate API Key
3. 复制保存密钥（格式：`nvapi-xxxx`）
4. 开始调用

没有绑卡、没有审核、没有等待。

## 代码示例

### Python（OpenAI SDK）
```python
from openai import OpenAI

client = OpenAI(
    api_key="nvapi-你的API密钥",
    base_url="https://integrate.api.nvidia.com/v1"
)

response = client.chat.completions.create(
    model="deepseek-ai/deepseek-v3.2",
    messages=[{"role": "user", "content": "用 Python 写一个快速排序"}],
    temperature=0.6,
    max_tokens=4096
)
print(response.choices[0].message.content)
```

### cURL
```bash
curl -X POST https://integrate.api.nvidia.com/v1/chat/completions \
  -H "Authorization: Bearer nvapi-你的API密钥" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "moonshotai/kimi-k2.5",
    "messages": [{"role": "user", "content": "你好"}],
    "temperature": 0.7,
    "max_tokens": 4096
  }'
```

### Node.js
```javascript
import OpenAI from 'openai';

const client = new OpenAI({
  apiKey: 'nvapi-你的API密钥',
  baseURL: 'https://integrate.api.nvidia.com/v1'
});

const response = await client.chat.completions.create({
  model: 'minimaxai/minimax-m2.7',
  messages: [{ role: 'user', content: '帮我写一个 React 组件' }],
});
console.log(response.choices[0].message.content);
```

## 接入开发工具

### Cursor / Windsurf
设置中添加自定义模型：
- Base URL: `https://integrate.api.nvidia.com/v1`
- API Key: `nvapi-你的密钥`

### Claude Code / Aider / OpenCode
```bash
export OPENAI_BASE_URL=https://integrate.api.nvidia.com/v1
export OPENAI_API_KEY=nvapi-你的密钥
```

## 注意事项

- **40 RPM 限制**：个人使用够了，需要更高可在 NVIDIA 论坛申请
- **模型可能临时下线**：多配几个备选
- **响应较慢**：轻量任务用 Nemotron Nano 或 Step Flash
- **数据隐私**：免费 API 可能收集使用数据，敏感信息别发
- **不建议生产环境**：40 RPM 和模型下线是风险，生产用官方付费 API

## 与其他免费平台对比

| 平台 | 模型数 | 额度 | 信用卡 |
|------|--------|------|--------|
| NVIDIA NIM | 100+ | 无限制 | 不需要 |
| Groq | 5-8 | 有速率限制 | 不需要 |
| Cloudflare AI | ~10 | 每天10K次 | 不需要 |
| OpenRouter免费版 | ~10 | 每天50次 | 不需要 |

## 适用场景

- 开发测试和原型验证
- 学习和体验不同模型
- 个人项目
- 对比模型效果（一个API Key调所有模型）
- 不想分别注册各家账号的开发者

## ⚠️ 注意
> 你的 OpenClaw 运行时就在使用 NVIDIA NIM 平台！当前模型 `nvidia/z-ai/glm-5.2` 就是走这个平台。
