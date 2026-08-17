# MiniMax Music 3 权重开源实测：57G 只用下一半，单卡就能跑，和作者那句"不保证"

> 来源：[微信文章](https://mp.weixin.qq.com/s/pj7ncY8_ulKRABYb7ET6Fw) · 2026-08-17

---

## 背景

一个能写满五分钟整曲的音乐大模型开源了权重。作者（配乐从业者）把 57.4G 拆开数了一遍，又把官方示范曲拉进分析器量拍速——两件事都跟通稿说的不一样。

起因：给客户做三十秒品牌片卡在配乐，客户要"没有版权纠纷、又不像罐头的曲子"。方向是"把作曲搬回自己能控制的地方"。

---

## 一、发布时间表：发布 ≠ 开源

- **Music 3.0** 七月十六号已作为云端 API 上线
- **八月十三号** 权重才放出来，中间隔四周

差别：API 意味着工程文件、未发布素材、创意方向每次生成都要过一遍别人的服务器；权重开源意味着机器可以摆在自己屋里。

另外：标题里的"海螺"是 MiniMax 视频线（Hailuo/H3）的招牌，这次开源的仓库从头到尾叫 `MiniMax-AI/MiniMax-Music3`，官方博客也没出现"海螺"二字。转发稿爱把"发布"和"开源"混着写。

---

## 二、57.4G 里有一半不用下

下载命令会把仓库 88 个文件、57.4G 全拉下来。但这是**同一个模型的两套打包，各存一份**：

- `qwen_7B/`：Global + Local 两个语言模型合成一个 checkpoint 的版本
- `language_model/` + `rvq_depth_decoder/`：同样两个东西拆开、按 diffusers 规则重新打包的版本
- `flowmatching_vae.pth` 和 `transformer/` 也是这个关系

**只下一条路，省 28.5G**。开源仓库体积别直接当"你要付出的代价"，打开文件清单数一遍常能数出一半水分。

---

## 三、"必须两张卡"：四份官方文档四个说法

| 来源 | 对显卡的说法 |
|------|-------------|
| GitHub README | 必须两张 CUDA 卡 |
| HuggingFace 模型卡 | 只要有 CUDA |
| SGLang-Omni cookbook | 单卡、双卡都给了命令 |
| 模型卡 diffusers 段 | 24G 单卡够；开 offload 后 8G 可跑 |

这些说法其实能圆上：GitHub README 描述的是 SGLang-Omni 默认双卡分工那条路，diffusers 那条路是 8 月 13 日才合进模型卡的。

**注意**：diffusers 这条路还挂在一个没合并的 PR 上（huggingface/diffusers#14456），得从指定 commit 装：
```bash
pip install git+https://github.com/huggingface/diffusers@dafe3733fcfdbf3c48915fe77be3aef65b5d6a2d transformers accelerate soundfile
```
能跑但没进主干，上生产得自己掂量。

> "必须几张卡"这种话最容易劝退人。看到这类话，先去翻推理框架自己的文档。

---

## 四、Hybrid-LM 架构：把"想旋律"和"出声音"拆开

| 模块 | 规模 | 职责 |
|------|------|------|
| Global LLM | 8B（Qwen3-8B 初始化） | 每帧只预测第一层码本，管整首歌走向（结构、副歌、桥段） |
| Local LLM | 0.6B | 补齐剩下七层码本，管音色、质感、齿音、拨弦颗粒 |

**为什么拆**：结构要在几分钟跨度保持一致，而一帧声音细节只跟前后几十毫秒有关。一个模型同时干两件事，要么记不住结构，要么细节糊掉。

**码本（RVQ）**：连续声音切成一帧帧，每帧在"音色字典"查条目号。第一本字典 16384 条管语义结构，后七本各 1024 条逐层补细节（残差矢量量化）。

**合成不走离散解码**：把 Global 和 Local 最后一层隐藏状态直接融合喂给合成器，绕开"码本条目号→波形"的查表（离散化必丢信息）。这也是它推理时不需要码本解码器的原因。

输出：32kHz、16bit 立体声 WAV。帧率 25fps，上限 9000 帧 = 360 秒（六分钟，比"最长五分钟"还留一分钟余量）。

---

## 五、作者自己写下的那句"不保证"（全文最有价值处）

Music 3 卖点"Fine-Grained Music Control"推荐写三段式结构化 Caption（Global Metadata 含 BPM/调号；Vocal Details；Arrangement），看着连 BPM 和调号都能指定。

但同份 README 最后一节 Limitations 第五条原话：

> Section tags and music descriptions provide **generative control rather than strict symbolic guarantees**. The generated tempo, key, instrumentation, lyrics, and song structure may not always match every requested detail exactly.

**你写的 BPM 和调号，它不保证给你，连歌词都不保证按你写的唱。**

### 5.1 官方示范曲跑丢了自己写的拍速
官方示范曲 `assets/minimax_ttm.wav` 的 Caption 第一行写 `bpm is 92. key is E, and scale is minor`（Electric Blues/Blues Rock）。四种方法实测：

| 方法 | 实测 BPM |
|------|---------|
| 自相关（onset envelope） | 117.5 |
| 梳状滤波扫频 | 119.4 |
| librosa 默认拍速估计 | 117.45 |
| librosa beat_track | 117.45 |

四个方法都指向 117–119，要求是 92，**差约 28%**。调号那栏倒是对的（E minor）。

### 5.2 61 首示范曲调性全量测
用 chroma_cqt + Krumhansl-Schmuckler 调性模板逐首比对，命中率 61%–74%。作者坦诚：自动调性识别本身有误差（真实音乐准确率本也就七八成），这个区间已贴着测量方法天花板，更像"调性基本跟得住"。

**拍速命中率**：三种估计器跑同一批 61 首，命中率 5%、13%、23%，差四倍多，作者判断是测量方法不够稳，故不给同口径数字。

---

## 六、官方通稿里没有 benchmark

官方发布博客一个数字都没有：无胜率、无 MOS、无 A/B 盲测、无生成耗时、无硬件环境，整篇是能力描述 + 示范曲。

> 一个把 "production-ready" 写进标题的发布，连一张对比表都不给，"它比谁强多少"只能靠自己下载权重去量。找不到出处的性能对比，默认当它不存在。

---

## 七、带走的认识

1. **"开源"和"发布"是两件事**：差四个星期和一整个使用场景。前者按次调用，后者摆进自己机房。
2. **仓库体积 ≠ 下载量**：57.4G 里两套打包各占一半，只需一条路 28.5G。"必须两张卡"是其中一条路的默认分工，换 diffusers 路 24G 单卡够，开 offload 8G 能动。
3. **作者写在 Limitations 的限制条款最诚实**：BPM、调号、歌词都做成可填参数，又写明"生成式控制，非符号级保证"。实测调性对、拍速差近三成。**能填的格子，不等于能兑现的承诺**——拿它做要卡节奏的正片配乐，得留返工余量。

---

## 参考链接

- GitHub 仓库：https://github.com/MiniMax-AI/MiniMax-Music3
- HuggingFace 模型与权重：https://huggingface.co/MiniMaxAI/MiniMax-Music3
- 官方示范曲试听页（61 首）：https://minimax-ai.github.io/music3-demo/
- 官方发布博客：https://www.minimax.io/blog/minimax-music-3-0-next-generation-open-weights-production-ready-versatile-music-model
- SGLang-Omni 推理 cookbook：https://sgl-project.github.io/sglang-omni/cookbook/minimax_music3.html
- ComfyUI 教程：https://docs.comfy.org/tutorials/audio/minimax/minimax-music-3