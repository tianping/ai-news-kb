# WorkBuddy 接入商汤 SenseNova 免费 API：三步手把手教程

> 来源：[WorkBuddy｜商汤大模型免费额度到手：0 元接进 WorkBuddy 手把手配（图文教程）](https://mp.weixin.qq.com/s/Qbh5hNRypNUCaRXHW_hJyw) · 智谱/AgnesAI/商汤 · 2026-08-21

## 概述

三步将商汤日日新 SenseNova 免费 API 接入 WorkBuddy，公测期间完全免费、不用绑卡、三个模型可选。

## 第一步：注册日日新，获取 API Key

1. 访问 https://platform.sensenova.cn/console
2. 手机号注册登录
3. 点击顶部「控制台」
4. 页面右侧显示可用免费模型：
   - `sensenova-6.7-flash-lite`
   - `sensenova-u1-fast`
   - `deepseek-v4-flash`
5. 点击「新增 API Key」，起个名字（如 workbuddy），点创建
6. 复制生成的 `sk-` 开头 Key（只显示一次，务必保存）

## 第二步：WorkBuddy 配置自定义模型

WorkBuddy → 左侧菜单「模型」→ 右上角「+ 添加模型」：

| 字段 | 值 |
|------|-----|
| 提供商 | 自定义 / Custom |
| 接口地址 | `https://www.sensenova.cn/` |
| API Key | 粘贴 sk- 开头的 Key |
| 模型名称 | `sensenova-6.7-flash-lite` |

高级配置：**必须勾上「工具调用」**，否则 agent 模式无法使用文件读写、搜索、执行命令等功能。

## 第三步：验证

回到主界面发消息测试，控制台 Token 用量从「暂无用量数据」变为有数据即成功。

## 三个模型怎么选？

| 模型 | 特点 | 推荐场景 |
|------|------|---------|
| sensenova-6.7-flash-lite | 轻量版，速度快、成本低 | 日常对话和写代码，推荐默认选这个 |
| sensenova-u1-fast | 通用型，能力比 flash-lite 强 | 需要更强推理时切换 |
| deepseek-v4-flash | DeepSeek 通过日日新通道免费用 | 习惯 DeepSeek 风格的用户 |

建议：先配 flash-lite 试手，不够用再换 u1-fast 或 deepseek。

## 踩坑提醒

1. **Key 只显示一次**：创建时没复制只能删掉重建
2. **接口地址别填错**：WorkBuddy 填 `https://www.sensenova.cn/`，不是 curl 示例里的完整 API 路径
3. **工具调用必须开**：不开文件读写、搜索、执行命令都会失效
4. **公测免费≠永久免费**：平台写的是"公测期间完全免费开放"，后续政策可能变

## 三家免费模型对比

| 平台 | 免费策略 |
|------|---------|
| 智谱 GLM | 给额度 |
| AgnesAI | 永久免费 |
| 商汤 SenseNova | 公测全免 |
