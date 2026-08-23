# Agnes AI 宣布三大模型 API 无限期免费开放

> 来源：微信公众号（老A） · [原文链接](https://mp.weixin.qq.com/s/WSkMTTWini43M2SS_ohWdw)
> 收录日期：2026-08-23

## 核心信息

Agnes AI 宣布**无限期免费开放**旗下文本、图像、视频三大核心模型 API，不限额度。唯一限制：每分钟请求数（RPM）≤ 20 次。

注意官方保留条款：可能根据基础设施容量、服务稳定性、模型可用性、滥用防范或产品策略进行调整——羊毛说不定哪天就没了。

## 免费开放的三大模型

| 模型 | 能力 | 亮点 |
|------|------|------|
| Agnes-2.0-Flash（文本） | 文本生成、代码、推理、Agent | 支持 1M 超长上下文 + 工具调用 |
| Agnes-Image-2.0-Flash | 文生图、图像编辑 | 进入 Artificial Analysis 图像榜单 |
| Agnes-Video-V2.0 | 文生视频、图生视频 | 支持音视频同步生成 |

新版文本模型 `agnes-2.5-flash` 也继续免费。

## 免费后数据量

- 首周文本调用量超 **1 万亿 Token**，图片生成超 200 万张，视频生成超 200 万秒
- 截至发文，单周 Token 处理量已飙到 **8.16 万亿**

## 接入三步走

API 兼容 OpenAI 风格接口，现有代码几乎不用改。

### 1. 注册 + 拿 Key

- 国际站：https://platform.agnes-ai.com
- 国内站（2026-07-29 启用）：https://agnes-ai.cn/ ，接口地址 `https://api.agnes-ai.cn/v1`
- 已有国际站账户的，把 Endpoint 从 `https://apihub.agnes-ai.com/v1` 换成 `https://apihub.agnes-ai.cn/v1` 即可

### 2. 直接调用（curl 示例）

```bash
curl https://api.agnes-ai.cn/v1/chat/completions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "agnes-2.0-flash",
    "messages": [{"role": "user", "content": "你好"}]
  }'
```

提示词结构建议：`[角色] + [任务] + [上下文] + [要求] + [输出格式]`

### 3. 接入 Agent 工具

已支持接入 Codex、OpenClaw、Hermes、Claude、WorkBuddy、OpenCode、Trae CN 等主流 Agent 工具。

官方文档标注上下文窗口 512K（约 40 万字），最大输出 65.5K；有说法输入支持 1M（未验证）。

## 实测反馈

- ✅ 一句提示词完成飞机大战网页游戏（完整玩法框架 + 连击提示、粒子爆炸、动态星空）
- ✅ 3D 农业大棚室内场景（番茄种植、抽风机、水帘墙、自动灌溉、开关按钮交互）
- ✅ 图像：赛博朋克雨夜东京，潮湿反光地面、霓虹光晕细节到位
- ✅ 视频：稳定生成上世纪电影风格
- ⚠️ 有开发者反馈效果距 Gemini / GPT 一线水平还有差距，但一般场合够用

## 点评

免费不是做慈善——Agnes 的算盘是用免费吸引开发者生态，未来靠 Pro 版付费盈利。但对独立开发者和小团队，现阶段调用成本门槛归零，试错空间极大释放。
