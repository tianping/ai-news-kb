# 用 AI 做一部手绘动画：从零到 40 分钟漫剧的完整工程实录

> 来源：[用 AI 做一部手绘动画：从零到 40 分钟漫剧的完整工程实录](https://mp.weixin.qq.com/s/E9tPLPu2p8RwwLSKgzsgxg) · 龙小儿886 · 2026-08-21

## 项目概述

将原创小说《深山古刹》改编为手绘 2D 暗黑动漫风格漫剧，使用 Agnes Image/Video API + edge-tts + ffmpeg，在 Windows 环境下完成全流程制作。

| 维度 | 数据 |
|------|------|
| 集数 | 5 集（EP1~EP5） |
| 总镜头数 | 568 镜 |
| 视频时长 | ~40 分钟 |
| 视频 API 调用 | 568+ 次 |
| 图片 API 调用 | 1100+ 次 |
| TTS 语音片段 | ~400 条 |
| 单集文件大小 | 50~205 MB |

### 技术栈
- 图片生成：Agnes Image 2.1 Flash
- 视频生成：Agnes Video V2.0（关键帧模式 keyframes）
- 配音合成：edge-tts（微软 Edge TTS，Python 异步调用）
- 视频处理：ffmpeg（合并、混音、帧抽取）
- 编程语言：Python 3
- 运行环境：Windows + PowerShell

## 核心工作流：链式帧动画

### 基本原理
每个分镜镜头 = 给定首帧 + 尾帧 → AI 在两帧之间生成过渡动画。上一镜的尾帧成为下一镜的首帧，链式传递。

### 每镜三步
1. **生成尾帧图片** — 文生图生成镜头结束画面
2. **首尾帧生成视频** — keyframes 模式，AI 在两帧间补全动画
3. **抽取实际尾帧** — 从视频文件抽取最后一帧，作为下一镜首帧（必须用实际尾帧，否则链式断裂）

### 链式验证
用 MD5 哈希前 12 位比对相邻镜头首帧与上镜尾帧，EP3（110 镜）和 EP4（138 镜）实现 0 链式断裂。

## 角色一致性：最难的工程问题

### 失败方案
- prompt 写"同一个角色" → AI 无记忆，每张图独立生成
- img2img 以角色参考图为底 → 姿态/场景带入新图，产生 AI 幻觉
- LoRA 训练角色模型 → 效果好但成本高，通用 API 不支持

### 最终方案：文字锚定常量 + text2img
为每个角色编写极其详尽的面部+服装文字描述，每次文生图拼接到 prompt 中。

**林远面部常量示例：**
```
black hair in tight round bun on top of head,
wine-red wide cloth headband across forehead with tail hanging on left side,
square jaw with strong mandible, high cheekbones,
thick black sword eyebrows with high peaks,
deep frown lines between brows,
narrow elongated eyes with slightly raised outer corners,
dark brown irises, weathered face, dark tanned wheat skin...
```

### 关键经验
- 描述要极其具体——AI 发挥空间越小，一致性越高
- 同时写 negative prompt，明确排除不想要的特征
- ECU 特写镜头必须包含完整面部描述
- 准备多版本常量（如含武器/无武器版），避免 API 内容过滤

### 画风锚定
每个 prompt 拼接全局画风描述：
```
hand-drawn 2D dark anime manga illustration,
ink outline contours, flat cel-shading with grainy texture,
dark gloomy aesthetic. Do NOT use 3D render, do NOT use smooth CG.
```

## 六大实战陷阱

### 陷阱一：手表问题
手腕特写几乎 100% 会被画现代手表，negative prompt 无法覆盖。
→ **解决**：彻底避开手腕区域，改为纯场景镜头或面部中景。

### 陷阱二：内容安全过滤
触发规律：
- "瞳孔收缩/放大" + ECU 特写 → 改面部 CU
- "透过缝隙偷看" + 人物 → 改"蹲在墙边"或纯场景
- "指甲抠入木头" → 改"手指紧握木板，指节发白"
- 武器描述 + 强烈情绪词 → 用无武器版常量
- "像蛇一样"的比喻 → AI 直接画蛇

### 陷阱三：现代角色污染
纯场景镜头中 AI 会自动生成穿现代连帽外套的人物。
→ **解决**：建立现代角色排除词集合（person, anime girl, modern clothes 等）。

### 陷阱四：数字控制极弱
prompt 写"exactly three monks"，AI 可能画 2 个或 5 个。
→ **解决**：改用空间构图描述（"front: two figures..., back center: one figure..."）。

### 陷阱五：比喻会被当真
写"like thin snake" → AI 直接画蛇。
→ **解决**：不想要的元素一个字都不要提，只写 negative prompt。

### 陷阱六：链式修复的级联灾难
第 50 镜需修复 → 第 51~110 镜首帧全部失效 → 级联重做。
→ **解决**：修复前评估影响范围，采用 chain_only 策略，保留无问题尾帧仅更新首帧+重做视频。

## 敏感内容"消毒"策略

| 原始描述 | 消毒后描述 |
|---------|-----------|
| blood | deep crimson stain / dark reddish-brown |
| corpse | pale shape in tattered white cloth |
| wound | injury mark |
| rotting | pale |
| bleeding | vermilion trace |
| horror | suspense |
| hammer（凶器） | heavy iron tool / iron shadow |
| nail piercing | metal marks on fabric / shadow falling |

EP5 SC.3~SC.6（116 镜，含铁锤钉麻袋、女尸现身、血泪等极端内容）全程采用消毒策略，0 次 content_policy_violation。

## TTS 配音与混音

### 角色声线分配（edge-tts）

| 角色 | 声线 | 调整 |
|------|------|------|
| 旁白 | zh-CN-YunjianNeural（低沉男声） | rate: -8%~-12% |
| 林远 | zh-CN-YunxiNeural（青年男声） | 愤怒时 +5% |
| 慧能 | zh-CN-YunyangNeural（中年男声） | rate: -5%（苍老伪善） |
| 疤脸僧人 | zh-CN-YunjianNeural | rate: +5%（凶狠粗野） |
| 女尸 | zh-CN-XiaomoNeural（低沉女声） | rate: -25%（空灵诡异） |

### 时间轴混音
ffmpeg adelay 滤镜定位到精确毫秒 → amix 混合所有片段（normalize=0 保持原始音量）。

## 工程化经验

1. **API 稳健性**：60→90→120→150→180→240s 递增等待重试；视频 API 90s 超时+5 次重试
2. **进程管理**：行缓冲实时日志；WMI 启动后台进程
3. **续传陷阱**：修复前必须手动删除旧 mp4/首帧/尾帧文件，否则续传会跳过
4. **文件损坏检测**：合并前用 ffmpeg 逐个探测，无 Duration 信息 = 损坏
5. **审查流程**：每镜抽取 5 个关键帧（10%/30%/50%/70%/90%）人工审查，A 类必修/B 类可接受

## 方法论总结

1. **AI 是执行者，不是创作者** — 分镜脚本、镜头衔接、节奏控制由人完成
2. **Prompt 是指令，不是文学** — 不写比喻，不描述不想要的元素
3. **链式传递是一致性的根基** — 实际尾帧成为下一镜首帧
4. **消毒是一种创作约束，不是限制** — 间接暗示比直接暴力更有悬念感
5. **工程化是可复制的前提** — 从重试策略到续传机制，每个细节决定能否持续推进
