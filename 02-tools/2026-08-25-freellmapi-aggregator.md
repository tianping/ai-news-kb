# FreeLLMAPI：19.9K Star，聚合 34 家 AI 免费额度约每月 74 亿 tokens

> 来源：[微信公众号](https://mp.weixin.qq.com/s/QhAO3mG5hZmQad_tlPPbcQ)
> 收录日期：2026-08-25
> 分类：工具与产品
> 项目：https://github.com/tashfeenahmed/freellmapi · 官网 https://freellmapi.co

## 一句话

跑在自己电脑上的「AI 接口聚合器 + 智能路由器」：把 34 家厂商的免费密钥收进来，对外只露一个统一接口地址和一把统一 Key（`freellmapi-xxx`）。程序只连这一个地址，它自动挑模型、避限流、失败换下一家。

像打车软件：你只管说去哪，平台自动派单给最好的司机；司机拒单（限流）秒换下一个，全程无感。

## 名片

- 19.9K Star · 2.9K Fork · MIT 协议（可商用）· TypeScript
- 创建于 2026 年 4 月，4 个月近 2 万 Star；最新 v0.8.7（2026-08-24）
- 支持 Windows / macOS / Linux / 树莓派 / 安卓
- 聚合规模：**34 家厂商 / 474 个模型家族 / 635 个免费端点 / 约 74 亿 tokens/月**
- Google、Groq、Cerebras、Mistral、OpenRouter、Cloudflare、Cohere、智谱 Z.ai、NVIDIA、HuggingFace、ModelScope（Qwen3、DeepSeek V4、GLM-5 都能白嫖）；还能接本地 Ollama、LM Studio

## 五大核心功能

1. **智能路由**：模型名写 `auto` 按实时速度/能力/成功率评分挑最优；`auto:fast` / `auto:smart` / `auto:reliable` / `auto:cheap`
2. **自动故障转移**：收到 429 自动冷却该密钥、换备用模型重试；同厂商多密钥轮换，每把密钥用量实时记账不撞免费上限
3. **密钥保险箱**：AES-256-GCM 加密存本地数据库，用时才内存解密；程序只见统一 Key，泄露风险大幅降低
4. **一条命令接入 16 个编程工具**：`npx freellmapi setup-claude` 三秒接好 Claude Code（改前自动备份）；另有 setup-codex/aider/cline/cursor 等
5. **中文管理面板**：模型排行、实时用量、成功率、延迟统计、Playground，支持 60 种语言

**彩蛋**：接口全——聊天/补全/embeddings/生图/生视频/TTS；兼容 Anthropic 格式（Claude Code 直接用）、Gemini 原生格式、Ollama 协议；「fusion」虚拟模型可一问多模型再由裁判合成最佳答案。

## 安装与使用

三种方式：

```bash
# Docker 一行命令（推荐）
curl -fsSL https://freellmapi.co/install.sh | bash
```

桌面版双击安装（Win .exe / macOS .dmg，常驻托盘）；安卓有 Google Play 官方 App 或 Termux 跑完整版。

三步上手：① 注册 2-3 家拿免费 Key（推荐先 Google AI Studio、Groq、智谱开放平台）→ ② 面板 Keys 页粘贴保存 → ③ 复制统一 Key 使用。

任何现成 OpenAI 程序改一行 `base_url="http://localhost:3001/v1"` 就能跑在免费额度上；响应头 `X-Routed-Via` 会告知实际服务方，挺透明。

## 短板（用前必读）

- 没有顶级旗舰，全是各厂免费档模型，别指望 GPT-5 级质量
- 免费档延迟波动大；白天尾部高峰好模型额度易耗尽（UTC 午夜重置）
- 免费版模型目录滞后 30 天（付费 $19/年当天同步——唯一付费项，路由器本体永久免费开源）
- 仅限个人实验定位，别拿来跑生产业务

## 适合谁

学生党/自学者（零成本摸 474 个模型家族）、独立开发者（免费额度够跑 MVP）、尝鲜党（一个接口横向对比各家）。不适合需要顶级质量和 SLA 的生产业务。

---

### 相关笔记

- [Free Claude Code：4.8 万 Star，50 多个平台免费额度收进一个界面](2026-08-25-free-claude-code-fcc.md)
- [DeepSeek 涨价后免费模型 API 盘点](../01-models/2026-08-20-deepseek-free-api-alternatives.md)
