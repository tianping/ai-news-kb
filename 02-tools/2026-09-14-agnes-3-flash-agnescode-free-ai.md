# OpenAI 关停 $200 Pro 套餐，Agnes 3.0 Flash + AgnesCode 全免费实测

> 来源：新智元（微信公众号）
> 原文：https://mp.weixin.qq.com/s/kYMoiUGsGhfAWbsPWPS_dw
> 收藏时间：2026-09-14
> ⚠️ 本文明显是 Agnes AI 推广软文，数字与结论需自己判断

## 背景

OpenAI 上周四宣布彻底关停 ChatGPT Pro $200 套餐，原因是 Astra 导致的算力需求太高、OpenAI 承受不了。开发者普遍抱怨试错成本太高。

## 主角

**Agnes 3.0 Flash + AgnesCode 桌面工作台**（Agnes AI 出品）

- AgnesCode：桌面 AI 工作台，整合本地项目、模型能力、Skills 扩展、MCP，支持 CLI 自然语言参与实际开发
- Agnes 3.0 Flash 已内置，API 缓存命中输入、输入、输出 Token 三项现价均 $0
- 定位：AI Coding、Agent、复杂任务工作流

## 实测三例

1. **照片→3D 老屋**：让 Agnes 先生成一张「1997 年前后中国北方城市普通人家客厅」，再让它根据照片复刻建模 3D 房间。WASD 走动、鼠标环视，灯绳按 E 会亮且微微闪烁，电视按 E 出雪花，冰箱门能拉，桌椅可搬、R 一键归位，全套音效（拉灯绳的"啪"、电视雪花噪声、冰箱门闷响）自动补齐
2. **火柴人 FPS**：Vite + Three.js 做第一人称射击游戏，横格纸风格，蓝色圆珠笔轮廓，敌人是红色火柴人；WASD + 鼠标瞄准 + 左键射击 + R 换弹，步枪/霰弹枪/武士刀手感不同，命中有墨迹效果，带血量/弹药/击杀/死亡重开
3. **HTML+SVG 鹈鹕骑自行车**（Simon Willison 的"照妖镜"梗）：单文件循环动画，鹈鹕坐稳车座、翅膀扶车把、双脚踩踏板、腿随踏板自然弯曲不脱节；模型自己意识到"扇翅膀"与"翅膀扶住车把"矛盾，主动砍掉，全程无人工干预

## 榜单与定价

- Artificial Analysis Index v4.3：Agnes 3.0 Flash 得 **36 分**，官方称与 DeepSeek V4 Pro 0813（max）持平
- 智能 × Token 价格：约 $0.03/M 进入帕累托前沿（软文口径，实际是"全免费"）
- 入口：[AgnesCode](https://agnes-ai.com/agnescode) / [开发者 API](https://platform.agnes-ai.com/login)

## 后续剧透

Agnes Harness（AGH）：开放 Agent 运行环境，计划 **9 月底开源**。统一 Runtime + Agnes Package（FDE）封装企业私有知识库/数据库/工作规范/验证规则，覆盖业务系统、Ai-for-Science、可编程硬件。

## 看点（自己的话）

- 免费 API + 桌面工作台（Skills + MCP + CLI）的组合，试错成本≈0，适合拿来做 Agent 工作流原型
- 三个实测都偏"演示向"（3D 房间、火柴人游戏、SVG 动画），真实项目里要看复杂任务交付质量，别只看 demo
- "AA 36 分、与 DeepSeek V4 Pro 持平"是软文说法，建议自己跑一遍再下结论
- 鹈鹕骑自行车梗：Simon Willison 每出个新模型都测一次，可以当小 benchmark 玩
