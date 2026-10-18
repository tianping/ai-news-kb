# Seedance 2.0 + OpenClaw：3 步做爆款短视频的技能流水线

- **日期**：2026-10-18（收藏）
- **来源**：微信公众号「灵犀眼阿成」
- **原标题**：3个步骤让Seedance视频秒变爆款，OpenClaw实操让效率翻倍
- **分类**：02-tools（AI 视频工具 / OpenClaw 技能）
- **URL**：https://mp.weixin.qq.com/s/fIwdOj7jclXMTl6U0eMjfg

## 概述

文章介绍用字节跳动 **Seedance 2.0**（文本→视频大模型）配合 **OpenClaw** 的一组视频类技能，把「选题 → 脚本 → 分镜 → 素材+配音 → 渲染 → 发公众号」串成一条流水线，号称自媒体一条视频从几天缩到半小时左右。偏教程/营销向，具体命令依赖文中提到的第三方技能，不是 OpenClaw 自带。

## 关键内容

### Seedance 2.0 的卖点（作者说法）
- **秒级生成**：提示词进去 1–3 分钟出可用视频
- **零门槛**：不需要剪辑基础，纯文字驱动
- **"爆款基因"**：作者称训练融入大量爆款视频特征（营销说法，无出处）

### OpenClaw 视频技能矩阵
| 技能 | 作用 | 调用 |
|---|---|---|
| video-director | 文字脚本 → JSON 分镜表；名词自动配 emoji/视觉元素，设计入场动画（pop-in、fade-slide-up） | `node scripts/plan.js "主题" '[{"text":"口播","emoji":"💡","title":"标题"}]'` |
| video-producer | 按分镜 AI 生成背景/图标素材、TTS 配音、Remotion 渲染合成，输出 `out/final.mp4` | `node produce.js "主题" '[...]'`（同样的 JSON） |
| seedance | 直接调用 Seedance 2.0 生成视频 | — |
| tavily-search | 选题阶段抓热点 | `python3 skills/tavily/scripts/tavily_search.py "今日AI热点" --topic news --max-results 5` |
| humanizer | 给 AI 文案去机器味 | — |
| wechat-publisher | 封面+文稿推公众号草稿箱 | `python3 scripts/publish.py --account main --input article.md --cover cover.jpg --title ... --digest ...` |

### 全链路 5 步（文中标题写 3 步，正文实为 5 步）
1. **热点选题 + 脚本**（10 分钟）：tavily-search 拿热点；脚本按「痛点开头 → 解决方案主体 → 互动引导结尾」
2. **画面规划**（5 分钟）：video-director 生成分镜表（时间轴、口播、布局、emoji 与动画类型）
3. **素材 + 配音**（10 分钟）：video-producer 自动出图、TTS、渲染
4. **人工微调**（可选，5 分钟）：剪映/Premiere 调字幕时间、加 BGM（注意版权）、检查跳切
5. **发布**（2 分钟）：wechat-publisher 推草稿箱

### 提示词模板
- **Seedance 场景**：现代办公室，年轻人坐电脑前，屏幕显示 AI 视频界面、数据增长曲线与特效文字"爆款视频"，落地窗外城市夜景，蓝紫科技光影，电影质感，4K
- **教程类分层画面**：左侧代码编辑器（Node.js 脚本），右侧手机演示生成效果，中间流程图连接，简洁科技风，蓝金配色

### 配图建议
- 封面：大字标题 + 痛点，左红「视频制作太难？」右绿「3个步骤秒变爆款」，底部两家 logo
- 正文 3 图：传统流程 vs AI 耗时对比、技能协作流程图（video-director → video-producer → 输出）、终端运行截图

## 备注

- 与本库 [Codex + MiniMax H3 + Seedance 视频工作流](2026-09-07-codex-minimax-h3-seedance-video-workflow.md) 同属"Agent + 视频模型"流水线思路；本篇用的是 Remotion 渲染 + TTS 的模板化口播视频，Seedance 只是可选底层。
- 本机已有类似能力：`story-to-handdrawn-video` 技能（Remotion 手绘故事视频）、`video_generate` 工具；文中的 video-director/video-producer/wechat-publisher 需从 ClawHub 另装，命令路径以各技能自身文档为准。
- "爆款基因""1 分钟成片"等属作者宣传说法，未核实。
