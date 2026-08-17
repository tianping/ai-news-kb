# 免费 API！DeepSeek V4 Flash 还能白嫖，AMD 日送 $10

> 来源：[免费 API！DeepSeek V4 Flash 还能白嫖，AMD 日送 $10](https://mp.weixin.qq.com/s/MLXRfE3vdJMPeIZVPSIqDA) · 2026-08-17 · 熊猫加电

AMD 上线了 **Token Factory**，把 DeepSeek V4 Flash 做成免费 API，每天白送 $10 额度，OpenAI 兼容，粘个 Key 就能用。

## 核心要点

- **免费模型不止 V4 Flash**：Qwen3.6-35B、MiniCPM 系列也全免，同一把 Key 通用，一共 **13 款**。
- **背景**：DeepSeek V4 Flash 官方 API 按量收费，个人偶尔玩成本心疼。AMD 这波对标 NVIDIA NIM 抢开发者生态。
- **入口**：`developer.amd.com.cn/radeon/tokenfactory`（国内镜像，邮箱注册，不用翻墙）。

## 三步白嫖

1. **注册登录** AMD 开发者账号，进入 Radeon Cloud。
2. **领 Key**：在「Public Free Model APIs」里找 `DeepSeek-V4-Flash-0731`，点一下生成 API Key（同一把 Key 调全部免费模型）。
3. **粘到 OpenAI 兼容工具**：base_url 填 AMD 接口地址，model 填 `DeepSeek-V4-Flash`。

```bash
curl https://developer.amd.com.cn/radeon/api/v1/chat/completions \
-H "Authorization: Bearer <你的Key>" \
-H "Content-Type: application/json" \
-d '{"model":"DeepSeek-V4-Flash","messages":[{"role":"user","content":"你好"}]}'
```

## 踩坑总结（实测）

| 坑 | 说明 | 对策 |
|------|------|------|
| 速度偏慢 | 首字延迟约 22 秒、输出 28 t/s，比官方慢 | 写稿、轻量任务够用；高频调用别指望 |
| 每日 $10 上限 | 额度用尽当天限流 | 重度需求错峰，或走官方付费 |
| 需注册账号 | 国内要邮箱注册登录 | 邮箱即可，无门槛 |
| 模型标 0731 | 快照版本可能随更新变动 | 接口兼容，换 model 字段即可 |

## 点评

- AMD 这波是在抢开发者生态（对标 NVIDIA NIM），短期免费大概率延续，早薅早享受。
- 别拿它当主力高频接口——慢是真慢，但白嫖跑跑 Prompt 测试、写写小脚本很香。
- 信息差就在这：别人还在花钱买 API，AMD 直接白给。
