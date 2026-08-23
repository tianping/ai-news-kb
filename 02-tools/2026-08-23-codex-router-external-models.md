# Codex Router：在 Codex 里用 Anthropic/Kimi/DeepSeek/Grok 等外部模型

> 来源：GitHub [duolahypercho/codex-router](https://github.com/duolahypercho/codex-router)
> 收录日期：2026-08-23

## 是什么

一个本地路由器，让 OpenAI Codex App/CLI（以及 DeepSeek Harness、Gemini CLI）直接调用外部模型——Anthropic Claude、Kimi、DeepSeek、xAI Grok、GitHub Copilot、MiniMax、小米 MiMo、通义 Qwen 等。

核心机制：路由器在本地运行一个后台服务，对外说 Responses API 协议，把外部模型条目合并进 Codex 原生模型目录。路由后的模型出现在 Codex 的模型选择器里，与原生 GPT 模型并列。同一套安装也可发布到 DeepSeek Harness 的 Models 页和 Gemini CLI。

## 关键特性

- **凭据隔离**：所有 provider 凭据只存在本地，不经过 Codex 原生层；Codex 登录态和原生 GPT 目录不被覆盖
- **一份安装多客户端共享**：一个后台服务 + 一套 provider 凭据，同时服务 Codex / Harness / Gemini CLI
- **模型策展**：`model-picker.json` 白名单控制哪些路由模型对外可见；provider/model 选择和 picker 可见性存在本地状态目录，重建后可恢复
- **安全迁移**：只识别已知旧版本才迁移，支持回滚；`codex-router doctor` 自检
- **无需在聊天里贴 key**：凭据通过隐藏终端提示输入

## 支持的模型/Provider（部分）

| Picker 标签 | Model ID | 认证方式 |
|---|---|---|
| K2.7 Coding Highspeed (OAuth) | kimi-oauth/kimi-for-coding-highspeed | Kimi Code CLI OAuth |
| Kimi K3 (OAuth/API/China API) | kimi-oauth/k3 等 | OAuth 或 API key |
| DeepSeek V4 Flash / Pro (API) | deepseek/deepseek-v4-flash | API key |
| DeepSeek V4 Flash/Pro (Ollama Cloud) | ollama-cloud/deepseek-v4-* | Ollama Cloud key |
| Grok 4.5 (OAuth/API) | grok-oauth/grok-4.5 | OAuth 或 xAI API key |
| Claude Opus 4.8 (API) | anthropic-api/claude-opus-4.8 | Anthropic API key |
| GLM-5.2 (Ollama Cloud) | ollama-cloud/glm-5.2 | Ollama Cloud key |
| MiniMax M3 (Ollama Cloud / Token Plan) | minimax-token-plan/minimax-m3 | 各自 API key |
| MiMo-V2.5 / Pro (Xiaomi API) | xiaomi-mimo/mimo-v2.5(-pro) | 小米 MiMo API key |
| Qwen3.8 Max (Plan) | qwen-plan/qwen3.8-max | 阿里 Model Studio plan key |

## 安装

```bash
# Homebrew
brew tap duolahypercho/codex-router https://github.com/duolahypercho/codex-router
brew install codex-router
codex-router setup --guided

# macOS/Linux 一键脚本
curl -fsSL https://raw.githubusercontent.com/duolahypercho/codex-router/main/install.sh | sh -s -- --target codex --guided
```

## 要求

- Codex App 或 CLI
- Node.js 22.19+（推荐 24 LTS）
- uv 或 Python 3.10+ with venv
- Git

## 收录理由

Codex 生态的重要补充工具：打破 Codex 只能用 OpenAI 模型的限制，一键接入主流国产+海外模型，对国内开发者尤其有吸引力（Kimi/DeepSeek/GLM/Qwen/MiniMax 全覆盖）。
