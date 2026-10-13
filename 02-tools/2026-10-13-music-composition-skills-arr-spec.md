# 开源专业级作曲编曲 Skills：music-composition-skills（ARR-SPEC 规格单驱动 Suno/YuE2）

- **日期**：2026-10-13
- **来源**：微信公众号「柯基爱好者的领域」
- **分类**：02-tools（Agent Skills / AI 音乐）
- **原文**：https://mp.weixin.qq.com/s/0_mBMYIWc_JtqmCVFGZCog
- **仓库**：https://github.com/jtydhr88/music-composition-skills （MIT；底本引文版权归原作者）

## 概述

29 个 agent skill（Claude Code 与 OpenAI Codex 通用，遵循 agentskills.io 开放规范），做流行音乐的作曲与编曲。知识底本是 **50 本**配器、和声、曲式、复调、混音与风格实务书，外加 **62 首**从 Cambridge-MT 多轨录音实测并反写的编曲语料。

核心思路：**不直接写提示词，而是先填一份 ARR-SPEC（编曲规格单）**——调、速度、精确到小节的段落表、字母和弦 + 罗马数字级数、乐器进退场、能量曲线、演唱方式、混音意图——再把这一份文档编译成 Suno 提示词、YuE2 乐谱（ABC）或可导入 DAW 的 MIDI/MusicXML。同一份规格单同时用作：对外交接、可控 A/B、编译源、验收基准。

代价是**用时间换质量**：完整走一遍一首歌常要 20 分钟以上。库内不含任何音乐作品。

## 为什么有了生成模型还需要它

- Suno/YuE2 能把一句话变成完整的歌，但缺"编曲决定"：没指定的部分模型用默认值补，于是听起来都像同一个制作人。
- 规格单交出的是决定本身：吉他第几小节退、副歌前先撤哪层、能量在哪下落、第三遍副歌人声有何不同。
- 一句提示词无法逐字段核对；规格单可以逐字段比对、一次只改一个字段重跑做对照，每个决定都带理由与出处。
- "模型仍是演奏者和录音棚，这份东西是递给它的编曲谱。"

## 四库体系（各自独立，经规格单交接）

| 库 | 作用 | 状态 |
|---|---|---|
| music-composition-skills | 作曲编曲 → ARR-SPEC | 已发布 |
| lyric-writing-skills | 作词 → LYR-SPEC（普通话/粤语/英/日/韩语言层） | 已发布 |
| 配乐库 | spotting、cue 写法、主题发展 | 待建 |
| 混音与声音设计库 | EQ/压缩/混响/自动化/母带 | 待建 |

ARR-SPEC 与 LYR-SPEC 共享六字段：段落 id、小节数、段落名、速度、调、谁在唱。**谁先做谁定**，后做方只读；要改就走消息通道请先做方改。姊妹项目：screenwriting-skills；本库取代 japanese-composition-skills。

## 六层架构（上层只知道下层，每个 skill 带"边界表"）

- **L0 工作流**：`mc-workflow`——路由、阶段表（立意→骨架→旋律和声→编制→演唱混音→编译→生成→验收）、ARR-SPEC 规范、后端能力档案、二十条自查、风格混合分工规则
- **L1 通用层**：mc-melody、mc-harmony、mc-progressions（122 条进行速查）、mc-modulation、mc-rhythm-groove、mc-form、mc-counterpoint、mc-development（十种动机发展手法）
- **L2 编曲层**：mc-arrangement-arch（能量曲线/减法事件，62 首验证）、mc-orchestration（含中国民族管弦）、mc-texture-layering（频谱预算/临界频带掩蔽）、mc-rhythm-section（十一张风格鼓型表）、mc-sound-design（99 条合成器配方）、mc-vocal-direction、mc-mix-intent
- **L3 风格层**（各带"移植表"）：jpop、citypop-rnb、jazz、latin、chinese-pop、edm、rock-band、cinematic、hiphop
- **L4 执行与检验**：mc-render-compile（一个编译器多后端，backends.yaml 中每字段声明 honors: exact/hint/none）、mc-symbolic-score（ABC/MusicXML/MIDI）、mc-ai-tell-audit（**十四条可枚举的 AI 味指纹**，与"是否照规格执行"分开评估）
- **L5 语料层**：mc-case-studies——让唱片给教材规则作证或证伪；语料自然抽样、不策展

## 安装

```bash
# Claude Code 插件市场（推荐）
/plugin marketplace add jtydhr88/music-composition-skills
/plugin install music-composition@music-composition-skills
# 调用：/music-composition:mc-workflow

# 或个人级手动安装
git clone https://github.com/jtydhr88/music-composition-skills.git
cp -r music-composition-skills/plugins/music-composition/skills/* ~/.claude/skills/

# Codex
codex plugin marketplace add jtydhr88/music-composition-skills
```

纯文本插件，无需 Python/Node，无脚本。

## 常见用例（brief 怎么说 → 走哪些 skill）

1. **参照某歌手风格写带词的歌**：lw-workflow→lw-song-intent→lw-structure→lw-mandarin→lw-tone-check；mc-workflow→mc-style-chinese-pop→mc-melody→mc-development→mc-arrangement-arch→mc-vocal-direction→mc-render-compile。出 LYR-SPEC + ARR-SPEC + Suno 材料包。参照只借编制/速度/人声人设/段落走法，不借词曲。
2. **抖音口水歌，不要叙事**：「不要叙事」决定歌词走态度型，否则默认写成情境。
3. **伤感情歌，直接说不要场景**：态度型无物件，锚是副歌反复的一句话。
4. **一种心情不写成故事**：情境型——有物件（如充电线），没有情节推进。
5. **器乐曲从零作曲**：判据在 mc-development——新材料不过半，有再现。
6. **已有词曲只做编曲**（如 salsa + 铜管）：只出 ARR-SPEC。
7. **风格融合**（主歌 rap + 副歌中国风）：mc-workflow §2.4，hiphop 为基底、chinese-pop 叠加。
8. **粤语填词**：lw-cantonese 先按音高类别算再选字。
9. **本地 YuE2**：加 mc-symbolic-score 编译成 ABC 谱（实验性，唱出词比例偏低）。

brief 里真正起作用的：歌手+歌名、男/女声与音色、出口（Suno/YuE2/只要规格单）、歌词类型措辞、时长、产物清单。

## 设计体例亮点

- 每条规则带**失效条件或自检动作**（"测不了的规则只是意见"）；不过某条要么改规格、要么写理由，写了理由的偏离即合法，不会卡编译。
- 两家说法冲突时**并排保留**并注明适用场合（如 Levine 和弦-音阶判据 vs 日式流行实务）。
- 和弦双记（字母 + 罗马数字，默认 C 调）；引用标出处页码。
- 与 agent 无关：skill 文件不提任何 agent 名、不用专有语法；SKILL.md 控制在 40 KB 内，长表放 reference.md。
- mc-workflow 目录下六份规格文件：ARR-SPEC.schema.md、ARR-SPEC.template.yaml、backends.yaml、example-01-citypop.yaml、example-02-fusion.yaml、example-00-ai-default.yaml（反面样本：无引导时模型的默认产出及其在哪八条上不过）。

## 底本书目（50 本，摘要）

配器（阿德勒、杨立青、Pejrolo & DeRosa、杨春林、朱晓谷、李民雄）；和声复调乐理（斯波索宾、勋伯格、Levine《The Jazz Theory Book》、任达敏、于苏贤、黄虎威等）；曲式与歌曲写作（吴祖强、李吉提、杨儒怀、卡库里斯等）；编曲制作混音（文海良、野崎貴朗、竹內一弘、Izhaki《混音指南》、DeSantis 等）；节奏风格（《The Drummer's Bible》、Groove Essentials、Rooksby《Riffs》、Charnas《Dilla Time》、Schloss《Making Beats》、《How to Rap》）；声学（Howard & Angus）；粤语流行史（黄志华）。

## 备注

- 这是"领域知识 → 结构化中间表示（spec）→ 编译到多后端"范式在 AI 音乐上的典型实践，与提示词工程相比可核对、可复现、可 A/B。
- 相关：本库其他 Agent Skill 笔记（漫画 Skill、REA 逆向 Skill、Text-to-Lottie Skill）。
