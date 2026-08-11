# 开源AI音乐三大模型提示词手册：ACE-Step 1.5 / HeartMuLa / Stable Audio 3

> 来源：微信公众号文章 · 2026-08-11 · [原文链接](https://mp.weixin.qq.com/s/nmJqycNZNJ_WNdgrn9ulpg)

## 背景

2026年夏天，AI 音乐迎来"Stable Diffusion 时刻"：三个开源模型各有绝活，覆盖带歌词歌曲和商用背景乐。

| 模型 | 参数 | 许可证 | 定位 |
|------|------|--------|------|
| ACE-Step 1.5 | 5B | MIT | 歌词→带人声完整歌曲（50+语言） |
| HeartMuLa | 3B/7B | Apache 2.0 | 标签+歌词控制，音乐理解与编辑 |
| Stable Audio 3 | 459M-2.7B | CC | 器乐/音效/氛围，版权最干净 |

---

## ACE-Step 1.5：标签公式 + 歌词结构

### 标签公式：8维模板

```
[genre], [sub-genre], [mood], [2-3 instruments], [vocal type], [production style], [era], [BPM] bpm
```

示例：`lo-fi hip-hop, boom bap, dusty drums, vinyl crackle, rhodes piano, male vocals, rap vocals, laid-back, warm, 90s, 88 bpm`

### 三条铁律

1. **乐器比形容词好用**：`grand piano, upright bass, brushed drums` 给模型具体靶子；`sophisticated, elegant` 模型不知道怎么执行
2. **标签上限12个**：超过12个关键词互相稀释，八精准好过二十模糊
3. **BPM双重设**：标签里写 `88 bpm`，参数里也传 88，两处一致锁节奏最稳

### 风格标签模板

| 风格 | 标签模板 |
|------|----------|
| 流行singer-songwriter | pop, singer-songwriter, acoustic guitar, piano, female vocal, breathy, intimate, warm analog, 72 bpm |
| 电子舞曲 | progressive house, euphoric, analog synth, 909 drums, female vocal, wide stereo, festival energy, 128 bpm |
| 中国风 | Chinese pop, guzheng, dizi flute, erhu, pipa, female vocal, ethereal, cinematic, 80 bpm |
| 电影配乐 | cinematic orchestral, epic, slow tempo, strings, brass, emotional, film score, 60 bpm |
| 爵士 | jazz, swing, saxophone, upright bass, brushed drums, male vocal, smoky, warm, 120 bpm |
| lo-fi说唱 | lo-fi hip-hop, boom bap, dusty drums, jazz sample, rhodes piano, male vocals, laid-back, 88 bpm |

### 歌词结构：方括号标记

段标（全部方括号小写）：
- `[verse]` 主歌 / `[chorus]` 副歌 / `[bridge]` 桥段
- `[inst]` 纯器乐段落（间奏/独奏）
- `[intro]` 前奏 / `[outro]` 尾奏 / `[pre-chorus]` 副歌前推升

行长度：4-8音节流动自然，超12音节会踩碎节奏
词速：每秒2-3词（47秒歌控制90-140词）

语言标记：`[en]` / `[zh]` / `[ja]` 放段首，支持19种语言

### Turbo→XL→Base 三阶工作流

1. **Turbo（8步）**：快速扫5-10版，找对味方向，每版不到一分钱
2. **XL Turbo**：出成片，标签歌词不动，画质提升
3. **Base（27步甜点）**：精修交付

CFG：5-7松而自然（抒情/民谣），10-12紧贴标签（电子/舞曲），超15有杂音。steps超60收益递减。

### 编辑模式四件套
- **Retake**：同样prompt出不同版本
- **Repaint**：重画指定秒区
- **Extend**：续时长
- **Remix**：换风格

### 常见翻车诊断

| 问题 | 原因 | 修法 |
|------|------|------|
| 人声压过伴奏 | 乐器标签太少 | 补具体乐器名 |
| 歌词挤在一起 | 字数太多或缺断句 | 按每秒2-3词砍，插[inst] |
| 风格乱炖 | 矛盾标签 | 清掉冲突，控制3-7个 |
| 副歌软 | 副歌行太短 | 4-6行、押韵AABB/ABAB |
| 发音不准 | 语言标记位置不对 | 放段首首位，单行≤10音节 |

---

## HeartMuLa：标签情绪矩阵 + 段标控制

### 标签写法：情绪驱动关键词矩阵

核心原则：「情绪 × 乐器 × 场景」三角对齐，逗号分隔。

示例：`piano, melancholic, rainy night, slow tempo, female vocal, dreamy, reverb`

与ACE-Step区别：不写BPM在标签里（有自己的 `--cfg_scale` 和 `--temperature`），情绪词权重更高，乐器权重更低。

### 标签模板

| 场景 | 标签模板 |
|------|----------|
| 婚礼 | piano, happy, wedding, strings, romantic, warm, slow tempo |
| 摇滚 | rock, energetic, guitar, drums, male vocal, aggressive, 140 bpm |
| 冥想 | ambient, peaceful, nature sounds, pad, no vocals, slow, meditative |
| 动漫OP | anime opening, j-pop, synth, electric guitar, upbeat, female vocal, 160 bpm |
| R&B | R&B, smooth, saxophone, electric piano, male vocal, sensual, 75 bpm |

### 歌词段标

与ACE-Step共用方括号段标，但 HeartMuLa 对段标顺序更敏感，建议经典结构：
`Intro → Verse → Chorus → Verse → Chorus → Bridge → Chorus → Outro`

tags文件独立于歌词，同一套歌词换不同tags出不同风格版本。

### 注意事项
- 标签偶尔不生效（GitHub #90），歌词权重比标签大
- 标签控制5-8个最有效，超10个命中率下降
- HeartCodec保持fp32，bf16降音质

---

## Stable Audio 3：纯Prompt驱动，零歌词

### Prompt公式

```
[genre], [instruments], [mood], [scene], [BPM]
```

示例：`ambient cinematic, slow strings, ethereal choir, floating through misty forest, 60 BPM`

**不支持歌词**，写"female vocal singing about love"不会出人声，只会出带人声质感的器乐。

### 场景模板

| 场景 | Prompt模板 |
|------|------------|
| YouTube背景乐 | lo-fi hip-hop, chill, mellow, piano, jazzy chords, vinyl crackle, 85 BPM |
| 播客开场/结尾 | upbeat instrumental, acoustic guitar, bright, podcast intro, 15 seconds |
| 恐怖/悬疑 | dark ambient, tension, low drone, metallic scrapes, horror atmosphere |
| 游戏战斗 | epic battle, orchestral, percussion, brass stabs, intense, 140 BPM |
| 自然纪录片 | nature documentary, orchestral, strings, woodwinds, sweeping, majestic, 80 BPM |
| 音效 | footsteps on gravel, close mic / thunder strike, low rumble, 3 seconds |

### 最大武器：版权

训练数据全部正版授权（806,284条AudioSparx + 472,618条Freesound CC），华纳和环球参与合作。商用背景乐被版权方追责风险为零。

---

## 三模型对照速查

| 维度 | ACE-Step | HeartMuLa | Stable Audio 3 |
|------|----------|-----------|-----------------|
| 标签格式 | 8维空格逗号分隔 | 逗号分隔情绪词串 | 自然语言描述 |
| 歌词支持 | ✅ 方括号段标+语言标记 | ✅ 方括号段标+独立tags文件 | ❌ 纯器乐 |
| BPM控制 | 标签内+参数双锁 | 参数控制 | Prompt内写 |
| 风格切换 | 改标签→新风格 | 同歌词换tags→不同版本 | 改prompt→新风格 |
| 最强武器 | 8维标签公式 | 同歌词多风格复用 | 版权最干净 |
| 建议标签数 | 8-12 | 5-8 | 不限越具体越好 |

## 高频标签清单

| 类别 | 标签 |
|------|------|
| 乐器 | acoustic guitar, piano, synth pads, strings, brass, saxophone, 808 drums, electric bass |
| 情绪 | melancholic, uplifting, energetic, dreamy, dark, nostalgic, euphoric, intimate |
| 人声 | female vocal, male vocal, breathy, powerful belting, falsetto, choir, whispered |
| 风格 | pop, rock, jazz, electronic, hip-hop, R&B, folk, lo-fi, cinematic |
| 制作 | warm analog, studio-polished, bedroom pop, live recording, wide stereo, reverb |
| 节奏 | slow tempo, mid-tempo, fast-paced, driving, laid-back, groovy |

## 一行部署

```bash
pip install ace-step && ace-step generate -l lyrics.txt -t pop,piano,female vocal -o song.mp3
```

## 核心观点

> 好听的AI音乐不是抽卡抽出来的，是标签公式写出来的。
