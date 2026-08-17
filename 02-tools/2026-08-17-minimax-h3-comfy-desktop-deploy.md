# MiniMax H3 + Comfy Desktop 本地部署全攻略：2K 视频 + 原生音效

> 来源：[微信文章](https://mp.weixin.qq.com/s/I_Tv5o7eI6UJIe2D93egNA) · 2026-08-17

---

## 概述

2026-08-03 MiniMax H3 正式开源权重。这是实打实能输出 **2K 分辨率、15 秒时长、带原生双声道立体声音频** 的商用级视频生成模型。

ComfyUI 官方在 H3 开源当天完成 Day-0 适配，推出原生工作流模板。配合 **Comfy Desktop**（官方桌面版），无需写代码、无需配 Python 环境，双击安装即可搭建本地 AI 视频工作站。

### 核心亮点
- ✅ **全模态统一理解**：文本、图像、视频、音频一个模型全搞定
- ✅ **原生音视频同步生成**：画面、对白、音效、配乐一次前向传播完成（非后期配音）
- ✅ **2K 分辨率直出**：2560×1440，24fps，最长 15 秒
- ✅ **视频编辑能力全球第一**：Artificial Analysis 榜单视频编辑项全球第一
- ✅ **RTX 3060 可跑**：INT8 量化 + 模型剪枝，内存占用从 123.6GB 压缩到 42.5GB
- ✅ **成本极低**：API 定价 0.8 元/秒（2K），约为同类旗舰模型 1/3

---

## Comfy Desktop 安装

1. 官网 `https://comfy.org/zh-CN/` 下载 Windows 安装程序，双击 `Comfy-Desktop-Setup.exe`（建议装非系统盘）
2. 首次启动选"本地"模式，勾选"启用镜像"加速下载
3. 更新到 **0.30.0+**（H3 需 ComfyUI 0.30.0 或更高）

### 模型文件目录
国内网络从 HuggingFace 下载会失败，改用魔搭社区手动下载：`https://modelscope.cn/models/Comfy-Org/MiniMax-H3`

安装初始化后，模型文件放 `D:\Comfy-Desktop\ComfyUI-Shared\models`，结构：

```
ComfyUI-Shared/
├── models/
│   ├── diffusion_models/
│   │   ├── minimax_h3_fl2va_pruned_int8_convrot.safetensors  (T2V/I2V, 19.5GB)
│   │   └── minimax_h3_ref2va_pruned_int8_convrot.safetensors  (R2V, 19.5GB)
│   ├── text_encoders/
│   │   └── qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors  (14.6GB)
│   └── vae/
│       ├── minimax_h3_video_vae_fp16.safetensors
│       └── minimax_h3_audio_vae_fp32.safetensors
```

**选型建议**：30/40 系显卡选 INT8；50 系 Blackwell 可选 NVFP4（体积更小、速度更快）。

### 工作流文件
H3 三种核心工作流：
| 工作流 | 说明 |
|--------|------|
| **T2V**（Text to Video） | 纯文本描述生成视频，适合创意发散 |
| **I2V**（Image to Video） | 图片让它动起来，支持首帧/尾帧控制 |
| **R2V**（Reference to Video） | 最强模式，最多 9 张参考图 + 3 段参考视频 + 3 段参考音频，锁定角色/风格/动作/声音 |

工作流 JSON 下载（GitHub workflow_templates 仓库），存放到：
`D:\Comfy-Desktop\ComfyUI-Installs\ComfyUI\ComfyUI\user\default\workflows`

---

## 实操要点

### 文生视频（T2V）
- 分辨率选择器把"百万像素"改成 0.9 → 生成 720p 视频（0.9 对应 720p）
- 修改提示词（含画面动作、音效、字幕、调色风格等），点运行即可

### 图生视频（I2V）
- **⚠️ 有个 bug**：分辨率选择器节点无论怎么设置都生成竖屏，需直接删除该节点
- 删除后默认变 1344×768（横屏）
- 输入提示词 + 负面提示词（变形、抖动、模糊、结构崩坏等）

---

## 总结

MiniMax H3 开源标志着"旗舰级视频生成"第一次真正走进个人创作者工作站。配合 Comfy Desktop 零代码工作流，一张消费级显卡即可产出带原生音效的电影级短片。

2026 年 AI 视频创作已从"能不能做"进入"做得多快、多便宜、多可控"的新阶段，H3 + Comfy Desktop 就是入场券。

> 注：本文与《MiniMax H3 本地部署保姆级教程》（2026-08-16）角度不同——那篇侧重 ComfyUI 命令行/SGLang 部署与显存优化、商用授权；本文侧重 Comfy Desktop 桌面版零代码安装 + 三种工作流实操 + 2K 直出。