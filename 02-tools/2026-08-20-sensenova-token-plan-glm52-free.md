# 疯了！商汤日日新把 GLM-5.2 也塞进免费池，Token 不限量

> 来源：[疯了！商汤日日新把GLM-5.2也塞进免费池，Token不限量](https://mp.weixin.qq.com/s/d26u7o5XNcXbTBr-um1dzA) · 2026-08-20

## 核心结论

**商汤日日新 Token Plan 公测免费，新增 GLM-5.2、DeepSeek V4 Flash、SenseNova 6.8 Flash Lite、SenseNova U1 Fast 四大模型，不限 Token 用量，仅限调用次数/5 小时窗口。**

## 免费模型额度一览

| 模型 | Model ID | 限额（5 小时窗口） |
|------|----------|-------------------|
| SenseNova 6.8 Flash Lite | `sensenova-6.8-flash-lite` | 1500 次 |
| SenseNova U1 Fast | `sensenova-u1-fast` | 1500 次 |
| DeepSeek V4 Flash | `deepseek-v4-flash` | 500 次 |
| **GLM-5.2** | `glm-5.2` | **500 次** |

**关键点**：各模型**单独计数**，四条线加起来约 4000 次/5 小时，对个人开发、Agent 试跑、小工具日用已属“准通勤车”级别。

## 3 分钟接入配置

**入口**：https://www.sensenova.cn/token-plan

1. 注册登录，控制台创建 API Key（`sk-` 开头）
2. 在工具中添加自定义模型（支持 OpenAI 协议的均可：Claude Code、各类 Agent、IDE 插件、工作流工具）
3. 配置四项：
   - 协议：OpenAI / Chat Completions
   - Base URL：`https://token.sensenova.cn/v1`（或 `/v1/chat/completions`）
   - 模型 ID：如 `glm-5.2`
   - API Key：控制台生成的 `sk-` 密钥

## 实战用法建议

1. **GLM-5.2 当免费主力脑**：长文本、复杂推理、项目级改码、Agent 规划
2. **DeepSeek V4 Flash 当高速副驾**：日常问答、快速改写、工具调用练手
3. **SenseNova 6.8 / U1 当多模态与办公爆发点**：文档、表格、图文处理
4. **接入 Hermes / OpenClaw 等 Agent**：官方文档明确提及支持，适合自动化工作流

## 风险提示

- **公测随时变规则**：模型会上下架、次数池会收、付费档位随时转正（页面已标注 Lite / Pro 即将上线）
- **正确姿势**：趁公测把工作流打通，**别把身家绑死在公测上**，Key 备份好迁移方案，重要业务留付费或自备通道
- 免费额度拿来放大实验和日常输出，**会吃免费红利的人，最后留下的是流程**

## 总结

| 项目 | 详情 |
|------|------|
| 平台 | 商汤日日新 Token Plan（公测免费） |
| 地址 | https://www.sensenova.cn/token-plan |
| 新亮点 | GLM-5.2 免费可调，同场还有 DeepSeek V4 Flash、SenseNova 6.8 / U1 |
| 规则 | 不限 Token，限次数；热门模型约 500 或 1500 次 / 5 小时，分开计算 |
| 接入 | `https://token.sensenova.cn/v1` + OpenAI 协议 + 控制台 API Key |
| 态度 | 现在猛用，同时准备好公测结束的后手 |

---

API 配置会的人，这波叫福利。API 配置不会的人，这波叫：眼睁睁看别人用免费旗舰干活。