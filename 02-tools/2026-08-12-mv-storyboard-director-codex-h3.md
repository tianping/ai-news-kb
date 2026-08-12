# MV导演开源Skill：Codex+MiniMax H3一首歌生成226秒完整MV

> 来源：[我把MV导演做成了开源Skill](https://mp.weixin.qq.com/s/jS5w_1eNIuGH4Q5N6ahmgg) · 2026-08-12

## 概述

作者「文哥」将完整 MV 制作流程拆成两个开源 Skill，配合 Codex 和 MiniMax H3，从一首歌生成 226 秒连续播放的完整 MV。不是十几秒的 demo 片段，而是从 0 秒到 226 秒的成片。

项目地址：https://github.com/penposs/mvmaker-h3-skills

## 核心挑战

AI 生成十几秒视频已不难，难的是让几十个镜头为同一首歌服务：
- 人物不能换脸
- 画面要跟歌词和节奏推进
- 片段间不能重叠或漏秒
- 生成失败后要知道从哪里继续
- 合片时每段视频要准确回到原曲位置

## 两个 Skill 分工

### mv-storyboard-director（导演）
- 分析歌曲、歌词、曲风和人物参考
- 判断适合叙事/表演/概念/混合结构
- 按歌词句末、换气和节奏变化拆分段落（10-15秒/段）
- 安排人物、场景、景别、运镜与蒙太奇
- 交付整数时间轴和四宫格分镜方案

### mvmaker-h3-skill（制片）
- 音频解码为 48kHz 双声道 PCM 母带，精确切片为 WAV
- 每段生成 16:9 四宫格故事板（作为 Picture 1 进入 H3）
- 调用 MiniMax 官方 H3 Prompt Skill 生成视频提示词（原样保存不改写）
- 通过 H3-GEN 提交到 RunningHub 任务队列（最多同时5段）
- 断点续传：读取已保存任务 ID，只补交失败或未提交片段
- 裁回整数时长、移除分段 AAC 音轨、按顺序拼接无声视频
- 最终只挂载一次标准母带，避免时间误差和音轨冲突
- 生成 HTML 制作档案（人物/分镜/提示词/任务记录/验证结果）

## 实战案例：《提示词之外》226秒MV

- 19 个片段，每段 10-15 秒，从 0 秒排到 226 秒
- 四个视觉人格：银白（接口）、猩红（拒绝爆发）、黑红（情绪）、骨白（觉醒）
- 四个人物始终保持各自的脸、发型和服装
- 最后一段副歌四人进入同一空间

## 安装使用

```bash
# 安装导演 Skill
npx skills add https://github.com/penposs/mvmaker-h3-skills --skill mv-storyboard-director

# 安装制片 Skill
npx skills add https://github.com/penposs/mvmaker-h3-skills --skill mvmaker-h3-skill

# 安装 MiniMax 官方 H3 Prompt Skill
npx skills add https://github.com/MiniMax-AI/MiniMax-H3 --skill h3-prompt-writing
```

准备歌曲、歌词、人物和场景参考，对 Codex 说：

```
使用 $mvmaker-h3-skill 完整制作这个 MV。
完成导演设计、故事板、官方 H3 Prompt、RunningHub 生成、
结果下载、验证、成片合成和 HTML 汇总。
```

## 环境要求

- Python 3、FFmpeg
- 本地 H3-GEN 和 RunningHub 配置
- 视频生成会产生 RunningHub 费用（提交付费任务前需授权）

## 亮点

- **整数时间轴**：切点贴近歌词句末和节奏变化，无重叠无空缺
- **断点续传**：中途退出后可恢复，不重复提交已计费任务
- **精确合片**：每段裁回整数时长，移除分段音轨，只挂载一次母带
- **四宫格故事板**：保持片段内四个关键状态的顺序性
- **HTML 制作档案**：每个镜头可回溯到原始输入
