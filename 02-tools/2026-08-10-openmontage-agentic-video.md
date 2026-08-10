# OpenMontage：全球首个开源 Agentic 视频生产系统

> 来源：[全球首个开源 Agentic 视频生产系统！](https://mp.weixin.qq.com/s/gIjq4bn0M5p4dldSqMjZsA) · 2026-08-10 · K的算法笔记本

## 一句话简介

GitHub 45K+ Star 的开源项目，用自然语言描述需求，AI Agent 自动完成从调研到渲染的全流程视频生产。12条流水线、100+工具、700+技能文件，零API Key也能跑通。

## 项目信息

- **项目链接**：https://github.com/calesthio/OpenMontage
- **协议**：AGPLv3
- **Star**：45K+
- **定位**：面向 Agent 的视频生产框架，把 AI 编程助手变成完整视频工作室

## 解决什么问题

传统视频制作需要脚本、素材、配音、剪辑、特效……每个环节都需要专业技能和不菲预算。Sora、Runway 等 AI 视频工具只生成片段，离真正视频制作还差很远。

OpenMontage 把零散的视频生产环节拼成可执行流水线——选题研究到最终渲染，脚本、素材、配音、字幕、合成全部由 Agent 接力完成。

## 核心原理

- **工具层**：100+ 生产工具，覆盖视频生成（14家提供商）、图像生成、语音合成（4种引擎）、后期处理（FFmpeg）、字幕生成等。每个工具是独立 Python 模块，统一接口。
- **流水线定义**：12条生产流水线，每条是 YAML 配置文件。如动画解说流水线：研究→脚本→图像→配音→合成→渲染。
- **Agent技能**：700+ 技能文件用 Markdown 写成，包含经验知识（检测音乐节拍、不规则图像适配16:9、质量检查等）。Agent 执行时自动检索相关技能。
- **双提供商策略**：每个能力同时支持云端API和本地开源方案（如TTS用ElevenLabs或免费Piper TTS离线），零API Key也能做真视频。
- **Backlot功能**：实时故事板面板，像看直播一样看视频制作过程——进度灯、脚本实时生成、场景卡片更新、费用明细。

## 12条流水线示例

- 动画解说流水线：研究→脚本→图像→配音→合成→渲染
- 纪录片蒙太奇流水线：素材检索→CLIP匹配→剪辑→配乐→渲染

## 安装与使用

### 环境要求
Python 3.10+、FFmpeg、Node.js 18+、一个AI编程助手（Claude Code / Cursor / Copilot / Windsurf / Codex）

### 标准安装
```bash
git clone https://github.com/calesthio/OpenMontage.git
cd OpenMontage
make setup
# 或手动:
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
cd remotion-composer && npm install && cd ..
python -m pip install piper-tts
cp .env.example .env
```

### 使用方式
在 AI 编程助手中打开项目，直接输入需求：
- "制作一个60秒的动画解说视频，讲解神经网络是如何学习的"
- "制作一部30秒吉卜力风格的动画视频，展示云端上一座神奇的漂浮图书馆"

### 可选付费API
- FAL_KEY — FLUX图像 + Google Veo、Kling视频
- PEXELS_API_KEY — 免费库存视频和图像
- SUNO_API_KEY — 完整歌曲、伴奏
- ELEVENLABS_API_KEY — 顶级TTS、AI音乐
- GPU本地视频生成：`make install-gpu`

## 适用/不适用场景

**适用**：教育内容创作者、市场营销人员、独立开发者/极客
**不适用**：专业电影/电视剧制作、需精准控制每帧的精品动画

## 常见踩坑

| 问题 | 原因 | 解决 |
|------|------|------|
| npm install 报 ERR_INVALID_ARG_TYPE | Windows兼容 | 用 `npx --yes npm install` |
| 无API Key时画质下降 | 免费工具分辨率有限 | 先验证流程再按需加付费API |
| 素材不符合预期 | 提示词不够具体 | 明确指定风格、色调、参考来源 |
| 字幕时间轴错位 | 语音识别精度 | 用更高质量TTS引擎（如ElevenLabs） |

## 总结

把"做视频"从专业工种拉到自然语言层面。对教育科普、产品宣传、社交媒体内容等"量大面广"的需求，提供了足够好、足够便宜、完全自主可控的开源路径。
