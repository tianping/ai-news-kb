# Agnes 2.5 Flash 免费接入 WorkBuddy：代码、生图、生视频一次打通

> 来源：[Agnes 2.5 Flash免费接入 WorkBuddy，代码、生图、生视频一次打通](https://mp.weixin.qq.com/s/_Tu1GUXxKJoFEvsdYTDAmA) · 2026-08-24

## 核心结论

Agnes AI 推出免费的 Agnes 2.5 Flash：面向智能体工作流、工具调用、编码和图像理解的快速模型，OpenAI 兼容接口，改 Base URL + API Key 即可接入 WorkBuddy。文本模型免费用，生图（agnes-image-2.1-flash）和生视频（agnes-video-2.5）单独配额。正好补上 Hy3 限免 8 月底到期后的免费通道空缺。

## 模型要点

- **512K 上下文**：整篇文档或中大型代码库一次丢进去
- **思考强度五档**：低 / 中 / 高 / 超高 / 极致；WorkBuddy 里可选推理模式与默认强度，深度推理拉"超高/极致"，日常跑"自动"
- **Claw-Eval Agent 实战榜**：超过 Gemini 3 Flash 和 MiniMax M2.7（该榜考察真实 Agent 任务：对话、推理、工具调用、执行）
- 定位：不替代主力模型，而是不烧积分的备用通道——主力排队时顶上，高倍率模型能省则省

## 接入步骤（约 10 分钟）

### 一、拿 API Key

- 打开 platform.agnes-ai.cn（无需翻墙），邮箱或手机号注册
- 创建新密钥，复制 `sk-xxxx`

### 二、WorkBuddy 配置

右下角 Auto → 「配置自定义模型」→ 提供商选「自定义/Custom」：

| 配置项 | 值 |
|--------|-----|
| 接口地址 | `https://apihub.agnes-ai.cn/v1` |
| API Key | 刚才的 sk-xxxx |
| 模型名称 | `agnes-2.5-flash`（全小写） |

工具调用、图片输入都勾选上，保存后发一句话测试即可。

### 三、顺手接生图 / 生视频

WorkBuddy UI 不能直接把图像/视频模型配成文本模型，官方办法是让它自己读文档打包成 Skill：

> 我想使用 Agnes Image 2.1 Flash 和 Video 2.5 模型生成图片和视频。请访问其 API 平台 https://agnes-ai.com/doc/overview 并将其打包为一个 Skill。

之后对话里贴画面描述即可出图/出视频。

## 免费的边界

- 网传"无限 token"不可信：无 SLA，高峰期可能排队，可能遇到 500/502 报错
- 生产项目需稳定付费通道：Agnes Pro 旗舰系列（1M 上下文），缓存命中 0.025 元/百万 Token，未命中 3 元/百万，输出 6 元/百万 ≈ DeepSeek V4 涨价前价格
- 推荐组合：日常轻任务走免费 Flash，复杂工程走付费 Pro

## 适合谁

高频写代码的开发者、需要 AI 配图但不想充会员的内容创作者、用 Agent 跑自动化工作流的个人用户。
