# Agnes 3.0 Flash 实测：免费模型又 +1

> 来源：[Leonn的手账](https://mp.weixin.qq.com/s/QDVhbilBtikN8Rj-HS0chA) | 作者：Leonn | 日期：2026-09-08

## 核心信息

- **定位**：面向 Agent 编程与工具驱动任务，卷的是「接到任务能不能真的干完、干对」
- **能力**：文本+图像 URL 输入，Function Calling、Thinking 模式，OpenAI/Anthropic 双协议兼容
- **免费 key 可用**，上下文沿用 2.5 flash 的窗口大小
- **两站不互通**：`apihub.agnes-ai.com/v1` 和 `api.agnes-ai.cn/v1`，key 长得一样但不通用，容易踩坑
- **Thinking 模式**需显式开启：`chat_template_kwargs.enable_thinking: true`

## 升级时间线

| 时间 | 版本 | 做了什么 |
|------|------|----------|
| 05.26 | 2.0 | 256K 上下文起步 |
| 06.01 | — | 全模态免费，首周文本 1T tokens |
| 07.13 | 2.5 | SWE-bench 72.4%→75.6%，SWE Atlas 近翻倍 |
| 09月 | 3.0 | 卷 Agent 执行：规划→调用→确认全链路 |

## 实测数据（同 key 同题，2.5 vs 3.0）

| 测试项 | 3.0 结果 | 用时 | 2.5 对照 |
|--------|---------|------|----------|
| 基础问答 | ✅ 答对且不啰嗦 | 6.5s | 答案也对，但思考 150 tokens 才开口 |
| Function Call | ✅ 正确调 get_weather，零废话 | 1.5s | — |
| 工具结果编排 | ✅ 拿数据直接总结，无重复调用 | 3.9s | — |
| 代码指令 | ✅ 说只要代码就真只给代码 | 6.6s | 同题 2.5 想了 52s 才开口 |

体感：3.0 默认带轻量内部思考（usage 里能看到 reasoning_tokens），输出极克制；工具编排拿到 JSON 直接总结收工，不重复调工具。

## 接入

```python
from openai import OpenAI
client = OpenAI(
    api_key="你的key",
    base_url="https://apihub.agnes-ai.cn/v1"
)
resp = client.chat.completions.create(
    model="agnes-3.0-flash",
    messages=[{"role": "user", "content": "你好"}]
)
print(resp.choices[0].message.content)
```

⚠️ 两站 key 不通用，混用直接报「无效的令牌」，按域名配对 key。

## 可信度说明

- 单次采样，非官方 benchmark
- 第三方测评暂为零，全网自称「3.0 深度评测」的可先划走
- 可参考 2.x 口碑：雷科技实测（问答出图够用）、PinchBench 榜单（2.0-Flash 60.9%，压 GPT 5.4 一头）
