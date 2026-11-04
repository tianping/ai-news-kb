# 仓颉.Skill 3.0：把任何视频（含画面）蒸馏成 Agent Skill

- **日期**：2026-11-04
- **来源**：微信公众号「袋鼠帝」
- **分类**：02-tools 工具与产品
- **原文**：https://mp.weixin.qq.com/s/IYURmdrA06amwqBQbyJAiQ
- **仓库**：
  - https://github.com/kangarooking/cangjie-skill （GitHub 12K+ Star）
  - https://github.com/kangarooking/kangarooking-skills/tree/main/video-downloader

## 概述

开源项目「仓颉 Skill」（cangjie-skill）升级到 3.0：配合重构后的 video-downloader，视频下载后不仅转写口播，还能识别画面里的操作、代码、图表、屏幕文字并按时间戳对齐，最后蒸馏成可装进 Agent 的 Skill 文件。作者用美团新发布的 LongCat-2.5 作为核心模型（既当 Codex 底座 Agent，又当视觉模型）做了三个实测。

## 分工

- **video-downloader**：下载视频、抽语音、解析画面，生成带时间戳的 `multimodal_transcript.md`（口播 + 画面事实 + 屏幕文字 + 动作 + 时间戳）
- **仓颉 Skill**：把文本提炼、结构化，蒸馏成 Skill（3.0 默认只产出 **一个** skill，不再拆成多个，方便管理）

## video-downloader 3.0 升级点

1. **视觉理解**：新增关键帧 / 完整视频 / Agent 智能选段三种模式
2. **Agent 选段**：先分析完整文案，再切重要片段交给视觉模型；每段最长 10 分钟，支持并发
3. **语音转写**：优先平台字幕 → 其次免费的硅基流动 SenseVoiceSmall → 本地 Whisper 仅在用户明确选择时使用
4. **视频号支持**：直接输入视频号链接，首次微信扫码登录后保存授权
5. **速度与稳定性**：默认 1080p，断点恢复 + 多级缓存，失败只重跑出错片段
6. LongCat-2.5 视频 API 只接受 URL → 借助一个可免费 API 上传视频、返回临时公网地址的站点中转

## LongCat-2.5（美团）要点

- 总参数 1.6 万亿，MoE，激活约 480 亿；全流程跑在国产 AI 芯片超节点集群上（文中称即将开源）
- **原生多模态**：RVQ 残差向量量化，图像在原生分辨率下离散成视觉 token，与文字同一语义空间（而非外挂视觉小模型转述）
- **长程任务**：1M 上下文；Trajectory2Task 机制——挖掘 Agent 失败轨迹、复原失败现场做针对性 RL
- W4A4 量化，推理吞吐提升 40%+
- OpenRouter 上匿名模型「Owl Alpha」即其 Preview 版，曾进调用量全球前三
- 价格：官方标价 百万 token 0.3 元；活动送 1000 万 token。作者三个视频蒸馏总共只用了约 755 万 token

## 三个实测

1. **B 站 4.5 小时长谈**（孙宇晨 × 邵艾伦）：下载约 1G，video-downloader 处理约 30 分钟（Cookie 抓 B 站 AI 字幕 + 关键画面），仓颉蒸馏约 50 分钟，得到「孙学决策 Skill」，问答能标注观点出处与前提
2. **视频号**（邵艾伦「薄肌 / 黄毛理论」）：扫码登录几分钟拿到原视频，蒸馏出「黄毛薄肌 Skill」；回答会主动补充常识（男性 13–15% 体脂、女性约 20%）
3. **YouTube 达利欧《经济机器是如何运行的》**：动画里大量图示（天平、信贷周期齿轮），视觉识别把图表关系切片注入文本；Skill 还能主动指出模型局限

## 上手三步

1. 在 Codex / Claude Code / WorkBuddy 里丢两个仓库链接，提示词：「帮我安装这两个 Skill，并检查 ffmpeg、yt-dlp、Python 3.10 以上环境和 PyYAML 有没有装好，没装的帮我全部装齐。」（9 月中旬更新过安装底包，老用户去 GitHub Release 重新拉）
2. 环境变量里配好所需模型的 API Key（缺失会提示）
3. 提示词：「用 video-downloader 处理这条视频：<链接>，并用仓颉蒸馏成专属 Skill。」

## 备注

- 长视频全流程仍需几十分钟到一两小时，偶有超时重连、格式兼容问题
- 文章有明显 LongCat 推广成分，性能/价格数字为作者自述
- 作者观点：蒸馏拿到的是别人的知识框架，替代不了自己的真实体验
