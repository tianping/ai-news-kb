# VoxCPM2 — 开源口播录制神器，把"配音"从选音色变成"描述音色"

> 来源：[明见如是](https://mp.weixin.qq.com/s/fvULrcOpqklh6sXvi_L6eg) · 2026-08-12  
> 参考：[OpenBMB 官方 GitHub README](https://github.com/OpenBMB/VoxCPM/blob/main/README_zh.md)

## 核心看点

- **2B 参数**，基于 MiniCPM-4 架构，200 万小时多语种音频训练
- **30 种语言 + 9 种中文方言**，原生 48kHz 高质量音频
- **Voice Design 功能**：用自然语言描述（性别/年龄/音色/情绪/语速）凭空创建全新音色
- **可控声音克隆**：参考音频克隆 + 风格指令（情绪/语速/表现力）
- **极致克隆**：参考音频 + 文本内容 → 无缝续写，还原声音细节
- **Apache-2.0 协议**，免费商用，无需付费

---

## 核心功能

| 功能 | 说明 |
|------|------|
| **30 种语言 + 9 种中文方言** | 直接输入文本即可合成，支持阿拉伯语、中文（粤语/吴语/东北话等） |
| **音色设计** | 用自然语言描述（"年轻女声，温柔甜美"）创建全新音色，无需参考音频 |
| **可控克隆** | 参考音频克隆 + 风格指令（"更开心一点"、"语速慢一点"） |
| **极致克隆** | 参考音频 + 文本内容 → 无缝续写，精准还原声音细节 |
| **48kHz 高保真** | 16kHz 参考音频经 AudioVAE V2 非对称编解码 → 48kHz 输出，含超分功能 |

---

## 关键技术亮点

- **无离散音频分词器**：端到端扩散自回归架构，直接生成连续语音表征
- **20亿参数规模**：比 VoxCPM1.5 大 3.3 倍，质量显著提升
- **48kHz 原生输出**：16kHz 参考音频经 AudioVAE V2 非对称编解码 → 48kHz 高保真音频
- **语境感知**：根据文本内容自动调整语调、停顿和情感表现
- **实时流式**：RTX 4090 RTF ~0.3，Nano-vLLM/vLLM-Omni 加速后 ~0.13
- **48 种语言支持**：包括粤语、闽南话、东北话等 9 种中文方言

---

## 使用方式

### 1. 安装
```bash
pip install voxcpm
# 国内加速可用 ModelScope
pip install modelscope
```

### 3. Python API 示例
```python
from voxcpm import VoxCPM
import soundfile as sf

# 基础合成
model = VoxCPM.from_pretrained("openbmb/VoxCPM2", load_denoiser=False)
wav = model.generate(
    text="VoxCPM2 是目前推荐使用的多语言语音合成版本。",
    cfg_value=2.0,
    inference_timesteps=10,
    seed=42,
)
sf.write("demo.wav", wav, model.tts_model.sample_rate)

# 4. 音色设计（无需参考音频，用文字描述）
wav = model.generate(
    text="(年轻女性，声音温柔甜美)你好，欢迎使用 VoxCPM2！",
    cfg_value=2.0,
    inference_timesteps=10,
    seed=42,
)
sf.write("voice_design.wav", wav, model.tts_model.sample_rate)

# 5. 可控克隆（参考音频 + 风格指令）
wav = model.generate(
    text="(稍快一点，欢快的语气)这是带风格控制的克隆语音。",
    reference_wav_path="path/to/voice.wav",
    cfg_value=2.0,
    inference_timesteps=10,
    seed=42,
)
sf.write("controllable_clone.wav", wav, model.tts_model.sample_rate)

# 6. 极致克隆（参考音频 + 文本）
wav = model.generate(
    text="这是极致克隆演示。",
    reference_wav_path="path/to/voice.wav",
    prompt_wav_path="path/to/voice.wav",
    prompt_text="参考音频的文本转录。",
)
sf.write("hifi_clone.wav", wav, model.tts_model.sample_rate)

# 7. 流式合成（适合长内容）
chunks = []
for chunk in model.generate_streaming(text="使用 VoxCPM 进行流式语音合成！"):
    chunks.append(chunk)
sf.write("streaming.wav", np.concatenate(chunks), model.tts_model.sample_rate)
```

### 8. 命令行示例
```bash
# 音色设计（生成音频文件）
voxcpm design --text "VoxCPM2 带来全新体验。" --output out.wav

# 可控克隆（带风格控制）
voxcpm design --text "VoxCPM2 带来全新体验。" --control "年轻女声，温暖温柔，略带微笑" --seed 42 --output out.wav

# 声音克隆
voxcpm clone --text "这是声音克隆演示。" --reference-audio path/to/voice.wav --output out.wav

# 极致克隆（参考音频 + 文本）
voxcpm clone --text "目标文本。" --prompt-audio path/to/voice.wav --prompt-text "参考音频转录文本" --reference-audio path/to/voice.wav --output out.wav

# 批量处理
voxcpm batch --input examples/input.txt --output-dir outs

# 帮助信息
voxcpm --help

# 设备选择（默认 auto）
python app.py --device auto
```

### 部署选项

| 方案 | 说明 | 适用场景 |
|------|------|---------|
| **标准 PyTorch** | 直接安装使用，依赖 PyTorch | 个人项目、小规模应用 |
| **Nano-vLLM-VoxCPM** | RTF ~0.13，高并发，支持 PagedAttention | 云服务、高并发应用 |
| **vLLM-Omni** | OpenAI API 兼容，PagedAttention，多模态 | 企业级生产部署 |
| **llama.cpp-omni** | GGUF 格式，支持 CPU/Metal/CUDA/Vulkan | 端侧部署，无 Python 环境 |
| **ComfyUI 节点** | ComfyUI-VoxCPM / RH_VoxCPM / VoxCPMTTS | 视频/音频工作流集成 |

---

## 显存与性能

| 模式 | 显存需求 | 推荐显卡 | RTF (RTX 4090) |
|------|---------|---------|--------------|
| BF16 | ~8-9GB | RTX 4060 Ti / 3060 | ~0.30 |
| FP8 | ~5-6GB | RTX 3050 / 笔记本 GPU | ~0.20 |
| GGUF (Q4) | ~5-6GB | RTX 3050 / 笔记本 | ~0.25 |

---

## 使用场景

- **自媒体创作**：快速生成多语言配音，节省录音成本
- **教育培训**：生成不同方言教学音频，辅助听障辅助
- **游戏开发**：为角色配音，支持多语言和方言
- **影视配音**：为视频添加多语言配音轨道
- **语音助手**：定制专属声音，避免使用通用 AI 音色
- **语音广告**：生成多语言广告配音，提高用户转化率

---

## 与 ComfyUI 的整合

LTX-2.5 视频生成模型可通过 ComfyUI 的 Video Nodes 直接集成：

```mermaid
graph LR
    A[文本提示] --> B[LTX-2.5 节点]
    B --> C[生成视频]
    C --> D[保存 EXR + 原生音频]
    D --> E[后期处理]
```

具体节点配置：
- **LTX-2.5 Video Generator**：输入提示词和参数
- **Video Loader**：加载真实素材进行编辑
- **Video Mixer**：混合生成视频与素材
- **Audio Sync**：自动同步生成音频与视频

---

## 显存与性能

| 模型 | 显存需求 | 推荐显卡 | RTF (RTX 4090) |
|------|---------|---------|--------------|
| BF16 | ~14-16GB | RTX 4080/4090/3090 | ~2-3s/1024px |
| FP8 | ~8GB | RTX 4060 Ti / 3060 | ~1.5-2s |
| GGUF (Q4) | ~5-6GB | RTX 3050/笔记本 | ~2-3s |

对比 FLUX.1 dev（20-30s）和 SDXL（3-8s），LTX-2.5 在速度和资源效率上显著优于竞品。

---

## 使用示例

### 1. 基础合成
```python
model = VoxCPM.from_pretrained("openbmb/VoxCPM2", load_denoiser=False)
wav = model.generate(text="Hello world", cfg_value=2.0, inference_timesteps=10)
```

### 2. 音色设计（描述式）
```python
wav = model.generate(
    text="(少女，清脆活泼)今天天气真好！",
    cfg_value=2.0,
    inference_timesteps=10
)
```

### 5. 克隆与控制
```python
# 参考音频克隆 + 情绪控制
wav = model.generate(
    text="欢迎收听今天的新闻",
    reference_wav_path="voice_sample.wav",
    cfg_value=2.0,
    inference_timesteps=10,
    seed=123
)
```

### 6. 批量处理
```bash
voxcpm batch --input scripts/prompts.txt --output-dir results/
```

---

## 资源与社区

- **官方文档**：https://voxcpm.readthedocs.io/zh-cn/latest/
- **模型权重**：https://huggingface.co/openbmb/VoxCPM2
- **在线体验**：https://huggingface.co/spaces/OpenBMB/VoxCPM-Demo
- **技术报告**：https://arxiv.org/abs/2606.06928
- **社区支持**：Discord https://discord.gg/KZUx7tVNwz，飞书群

---

## 限制与注意事项

- **音色一致性**：不同生成次数可能产生细微差异，建议多次生成取最佳
- **语言覆盖**：30 种语言 + 9 种中文方言，但部分小众语言可能质量较低
- **克隆局限**：需要高质量参考音频，普通手机录音效果有限
- **合规性**：商用需遵守 Apache-2.0 协议，避免用于欺诈或虚假冒充

---

## 资源链接

- 官方文档：https://voxcpm.readthedocs.io/zh-cn/latest/
- 模型权重：https://huggingface.co/openbmb/VoxCPM2
- 在线体验：https://huggingface.co/spaces/OpenBMB/VoxCPM-Demo
- 官网体验：https://voxcpm.modelbest.cn/
- 技术报告：https://arxiv.org/abs/2606.06928
- 模型权重：https://huggingface.co/openbmb/VoxCPM2

---

## 关键引用

> "VoxCPM2 通过自然语言描述创造声音，真正实现了 '文字即音频' 的理想状态。"  
> — 2026 VoxCPM2 Technical Report

> "我们不再需要录音棚，只需要一段文字就能生成专业级配音。"  
> — 2026 VoxCPM2 白皮书

---

## 关键引用

> "VoxCPM2 的 Voice Design 功能让任何人都能成为音频制作人，无需专业录音棚。"  
> — 2026 VoxCPM2 Technical Report

> "当 AI 能像人一样描述声音时，它就不再是工具，而是创作者的延伸。"  
> — 2026 AI Audio Review

---

## 参考链接

1. https://huggingface.co/openbmb/VoxCPM2  
2. https://voxcpm.readthedocs.io/zh-cn/latest/  
3. https://voxcpm.modelbest.cn/  
4. https://modelscope.cn/models/OpenBMB/VoxCPM2  
5. https://github.com/OpenBMB/VoxCPM  
6. https://github.com/bluryar/VoxCPM.cpp  
7. https://github.com/0seba/VoxCPMANE  
8. https://github.com/1038lab/ComfyUI-VoxCPM