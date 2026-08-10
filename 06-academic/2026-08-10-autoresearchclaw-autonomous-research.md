# AutoResearchClaw：全自主研究系统（从想法到论文）

> 来源：[GitHub - aiming-lab/AutoResearchClaw](https://github.com/aiming-lab/AutoResearchClaw) · 2026-08-10

## 一句话简介

全自主23阶段研究流水线，从一个研究想法到会议级论文（NeurIPS/ICML/ICLR）。支持完全自动或 Co-Pilot 协作模式，真实文献检索、沙箱实验、多智能体审稿、LaTeX 输出。

## 项目信息

- **GitHub**：https://github.com/aiming-lab/AutoResearchClaw
- **论文**：https://arxiv.org/abs/2605.20025
- **ARC-Bench**：https://huggingface.co/datasets/AIMING-Lab-UNC/ARC-Bench（55个主题的开放式自主研究基准）
- **协议**：开源
- **最新版本**：v0.5.0（2025-05-19）

## 核心能力

| 能力 | 说明 |
|------|------|
| 🧑✈️ Co-Pilot 模式 | 6种干预模式（全自动/门控/检查点/逐步/协作/自定义），SmartPause 自动检测需要人工输入的节点 |
| 🔄 PIVOT/REFINE 循环 | Stage 15 自主决策：PROCEED / REFINE（调参）/ PIVOT（换方向），产物自动版本化 |
| 🤖 多智能体辩论 | 假设生成、结果分析、同行评审均使用多视角结构化辩论 |
| 🧬 自学习 | 每次运行提取经验教训（决策理由、运行时警告、指标异常），30天时间衰减，未来运行从过去错误中学习 |
| 📚 知识库 | 每次运行构建6类结构化知识库（决策/实验/发现/文献/问题/评审） |
| 🛡️ Sentinel 看门狗 | 后台质量监控：NaN/Inf 检测、论文-证据一致性、引用相关性评分、防伪造 |
| 🔍 声明验证 | 提取 AI 生成文本中的声明，交叉比对已收集文献，标记无据引用和编造数字 |
| 🌿 分支探索 | 分叉流水线同时探索多个研究方向，并排比较结果，合并最优路径 |

## 23阶段流水线产出

- **paper_draft.md** — 完整学术论文（引言、相关工作、方法、实验、结果、结论）
- **paper.tex** — 会议级 LaTeX（NeurIPS/ICLR/ICML 模板）
- **references.bib** — 真实 BibTeX 引用（来自 OpenAlex、Semantic Scholar、arXiv）
- **verification_report.json** — 4层引用完整性+相关性验证
- **experiment runs/** — 生成代码 + 沙箱结果 + 结构化 JSON 指标
- **charts/** — 自动生成条件对比图（含误差线和置信区间）
- **reviews.md** — 多智能体同行评审
- **evolution/** — 自学习经验教训
- **deliverables/** — 所有最终输出，可直接编译

## 安装与使用

```bash
# 1. 克隆安装
git clone https://github.com/aiming-lab/AutoResearchClaw.git
cd AutoResearchClaw
python3 -m venv .venv && source .venv/bin/activate
pip install -e .

# 2. 设置（交互式，安装 OpenCode beast mode，检查 Docker/LaTeX）
researchclaw setup

# 3. 配置
researchclaw init  # 交互式：选择 LLM provider，创建 config.arc.yaml

# 4. 运行
export OPENAI_API_KEY="sk-..."
researchclaw run --config config.arc.yaml --topic "Your research idea" --auto-approve
```

### Co-Pilot 模式
```bash
researchclaw run --topic "Your research idea" --mode co-pilot
```

## OpenClaw 集成

AutoResearchClaw 兼容 OpenClaw，可直接通过聊天启动研究：
1. 把 GitHub 仓库 URL 分享给 OpenClaw
2. OpenClaw 自动读取 RESEARCHCLAW_AGENTS.md
3. 说："Research [你的主题]"
4. OpenClaw 自动克隆、安装、配置、运行、返回结果

### 桥接适配器（6项可选能力）
- `use_cron` — ⏰ 定时研究运行
- `use_message` — 💬 进度通知（Discord/Slack/Telegram）
- `use_memory` — 🧠 跨会话知识持久化
- `use_sessions_spawn` — 🔀 并行子会话处理并发阶段
- `use_web_fetch` — 🌐 文献综述时实时网络搜索
- `use_browser` — 🖥️ 浏览器论文采集

## 跨平台支持

支持任何 ACP 兼容的编码代理作为 LLM 后端（无需 API Key）：
- Claude Code、Codex CLI、Copilot CLI、Gemini CLI、Kimi CLI
- 通过 OpenClaw 桥接到 Discord、Telegram、飞书、微信

## 多领域实验代理（v0.5.0）

实验阶段自动按研究领域路由到专门代理：
- **高能物理**：ColliderAgent（Lagrangian → FeynRules → MadGraph5 → Delphes）
- **生物学**：COBRApy 基因组规模代谢建模
- **统计学**：仿真研究代理
- **化学/材料**：通用 Docker 执行器

## 版本历程

| 版本 | 日期 | 核心更新 |
|------|------|----------|
| v0.1.0 | 03/15 | 23阶段全自主研究流水线发布 |
| v0.2.0 | 03/16 | 多智能体子系统（CodeAgent/BenchmarkAgent/FigureAgent）、Docker沙箱、4轮论文质量审计 |
| v0.3.0 | 03/17 | MetaClaw 跨运行学习（+18.3%鲁棒性） |
| v0.3.1 | 03/18 | OpenCode Beast Mode、Novita AI 支持 |
| v0.3.2 | 03/22 | 跨平台支持、CLI代理代码生成、防伪造系统 |
| v0.4.0 | 04/01 | Human-in-the-Loop Co-Pilot 系统（6种干预模式） |
| v0.5.0 | 05/19 | 多领域实验代理 + ARC-Bench（55主题基准） |

## Skills Library

支持加载开源和自定义 Skill 增强研究体验，预装20个 Skill（科学写作、实验设计、化学、生物等），含社区贡献的 [A-Evolve](https://github.com/A-EVO-Lab/a-evolve) 代理进化 Skill。通过 `researchclaw skills install` 或放入 `.claude/skills/` 加载。
