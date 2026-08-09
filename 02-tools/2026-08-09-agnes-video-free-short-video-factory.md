# 薅羊毛：基于 Agnes Video 全免费制作的短视频工厂 SKILL

> 收藏来源：微信公众号「梁达标网络服务」（作者：liangdabiao / 梁达标）
> 原文链接：https://mp.weixin.qq.com/s/BWKcbn4TlIEGJhEC6CZNRw
> 收藏日期：2026-08-09

---

## 项目简介

一套**零成本**的 AI 短视频批量生成方案，核心思路是利用 **Agnes AI** 提供的免费 API，配合开源工具搭建全自动视频生成流水线。从文字剧本到成片，全程不花一分钱。

文章标题中的"薅羊毛"指的是充分利用 Agnes AI 的免费额度，"短视频工厂"则强调批量、自动化生产的能力。

## 核心三件套架构

```
Agnes AI（免费模型服务）
  ↓ API 调用
O4OpenAI（协议转换网关，localhost:1241）
  ↓ OpenAI 兼容接口
ArcReel / Agnes Video Generator（视频生成工作台）
```

### 1. Agnes AI — 免费模型服务

- 官网：https://agnes-ai.com/
- 注册即送免费额度，日常使用足够，无使用量上限（16 req/min 速率限制）
- 三个产品线：
  - **Agnes**：对话与推理，支持 Thinking 模式和 Function Calling，用于剧本创作、角色分析
  - **Echo**：图像生成（agnes-image-2.1-flash），文生图、图生图
  - **Pavo**：视频生成（agnes-video-v2.0），文生视频、图生视频、首尾帧生视频
- API 格式与 OpenAI 类似但有差异，需要中间件转换

### 2. O4OpenAI — 协议转换网关

- GitHub：https://github.com/javpower/O4OpenAI
- 作用：把 Agnes AI 的 API 翻译成 OpenAI 兼容格式，让不支持 Agnes 原生接口的工具也能调用
- 对外提供两套接口：
  - OpenAI 兼容：`/v1/chat/completions`、`/v1/images/generations`、`/v1/videos`
  - Anthropic 兼容：`/v1/messages`
- 关键特性：
  - 流式传输（SSE）
  - 模型映射（gpt-4o → Agnes 模型名）
  - Thinking 模式透传
  - Function Calling / Tool Use 完整支持
  - 图生图、首尾帧视频封装为 OpenAI 风格
  - Multipart 表单支持，兼容 OpenAI Python SDK

### 3. ArcReel — 小说变视频工作台

- GitHub：https://github.com/ArcReel/ArcReel（2.5k+ star）
- 核心能力：把小说/文本自动变成短视频
- 全流程自动化：
  1. 丢一段小说文本进去
  2. 自动提取角色、线索、场景
  3. 按集规划，生成剧本 JSON
  4. 给每个角色出设计图（保证跨镜头一致性）
  5. 生成分镜图 / 宫格图
  6. 每个分镜生成视频片段
  7. FFmpeg 合成完整视频，可导出剪映草稿
- 基于 Claude Agent SDK，多智能体架构（大 Agent 调度 + 小 Agent 各干各活）
- 支持供应商：Gemini、火山方舟、Grok、OpenAI、Vidu + 自定义供应商

## 部署步骤

### 第一步：获取 Agnes AI Key
- 去 agnes-ai.com 注册，进控制台生成 API Key（免费，不用绑卡）

### 第二步：部署 O4OpenAI
```bash
git clone https://github.com/javpower/O4OpenAI.git
cd O4OpenAI
# 按项目 README 配置 .env，填入 Agnes AI 的 Key
# 启动服务，默认跑在 1241 端口
```
验证：
```bash
curl http://localhost:1241/v1/models \
  -H "Authorization: Bearer YOUR_AGNES_API_KEY"
```

### 第三步：部署 ArcReel
```bash
git clone https://github.com/ArcReel/ArcReel.git
cd ArcReel/deploy
cp .env.example .env
docker compose up -d
```
- 访问 Web UI，默认账号 admin 登录

### 第四步：在 ArcReel 配置自定义供应商
- Base URL：`http://localhost:1241/v1`（指向 O4OpenAI）
- API Key：你的 Agnes AI Key
- 供应商类型：OpenAI 兼容
- ArcReel 自动调 `/v1/models` 发现可用模型

### 第五步：开干
新建项目 → 丢小说文本 → 选自定义供应商 → Agent 自动跑完全流程

## 相关项目：Agnes Video Generator

- GitHub：https://github.com/lcy362/agnes-video-generator
- 同样基于 Agnes AI 免费 API，功能更全面：
  - 文生视频、图生视频、关键帧动画
  - AI 旁白（TTS）+ 自动字幕（词级 SRT）
  - 数字人主播
  - 多场景流水线（创意模式 / 稿件模式）
  - 断点续传
  - Web UI（多语言）
  - 支持 9:16 / 16:9 / 1:1
  - 无水印
- 部署方式：手动（start.sh）/ Docker / npm（npx free-short-video）/ AI-Agent 辅助
- MIT 许可证，永久免费开源

## 竞品对比

| 特性 | Agnes Video Generator | Runway Gen-3 | Pika 2.0 | OpenAI Sora | Kling 1.6 |
|------|----------------------|-------------|----------|------------|-----------|
| 价格 | 免费 | $15–$95/月 | $10–$95/月 | $20+/月 | 免费额度后按秒付费 |
| 开源 | ✅ MIT | ❌ | ❌ | ❌ | ❌ |
| 自托管 | ✅ | ❌ | ❌ | ❌ | ❌ |
| 多场景流水线 | ✅ 内置 | ❌ 需手动剪辑 | ❌ | ❌ | ❌ |
| AI 旁白 | ✅ 免费 | ❌ 第三方 | ❌ | ❌ | ❌ |
| 自动字幕 | ✅ 词级 SRT | ❌ | ❌ | ❌ | ❌ |
| 数字人 | ✅ | ❌ | ❌ | ❌ | ❌ |
| 使用限制 | 无（16 req/min） | 按算力计费 | 按生成计费 | 按生成计费 | 按生成计费 |

## 实际体验

- **图像质量**：agnes-image-2.1-flash 出图快，质量中上；角色一致性场景建议多生成几次挑最稳定的
- **视频生成**：agnes-video-v2.0 异步任务，需轮询状态（O4OpenAI 已封装好）
- **角色一致性**：ArcReel 的角色设计图机制能保证跨镜头一致性
- **成本**：全程零成本，唯一开销是跑服务的机器（一台普通云服务器即可）
- **部署时间**：约 2 小时（大部分在改配置）
- **生成速度**：千字小说 → 视频，约十几分钟

## 适合人群

- 网文作者：把小说片段变短视频发抖音/B站
- 独立开发者：快速做产品 demo 视频
- 内容创作者：批量生产短视频内容
- AI 爱好者：学习多 Agent 协作、API 网关技术

## 相关链接

- Agnes AI 官网：https://agnes-ai.com/
- Agnes Video Generator：https://github.com/lcy362/agnes-video-generator
- 在线体验：https://video.lichuanyang.top
- ArcReel GitHub：https://github.com/ArcReel/ArcReel
- O4OpenAI GitHub：https://github.com/javpower/O4OpenAI
- Short Video Factory（桌面端工具）：https://github.com/YILS-LIN/short-video-factory
