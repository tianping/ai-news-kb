# NVIDIA Build Skills 总览

> 来源：[NVIDIA Skills 官方文档](https://docs.nvidia.com/skills) · [GitHub - nvidia/skills](https://github.com/nvidia/skills) · [build.nvidia.com/skills](https://build.nvidia.com/skills) · 2026-08-08

## 什么是 NVIDIA Agent Skills

NVIDIA Skills 是**便携的指令集**，教 AI 代理（如 Claude Code、Cursor、Codex、OpenClaw 等）如何正确使用 NVIDIA 软件。涵盖 Physical AI/机器人、仿真、CUDA-X 库、RAG 和 AI Blueprints、平台工具等。

- **GitHub**：https://github.com/nvidia/skills
- **文档**：https://docs.nvidia.com/skills
- **目录**：https://build.nvidia.com/skills
- **规范**：基于开放的 Agent Skills 规范（agentskills.io）

## 安装方式

```bash
# 交互式安装
npx skills add nvidia/skills

# 指定技能安装
npx skills add nvidia/skills --skill cuopt-numerical-optimization-api --yes

# 指定目标代理
npx skills add nvidia/skills --skill cuopt-numerical-optimization-api --agent claude-code
npx skills add nvidia/skills --skill cuopt-numerical-optimization-api --agent codex
npx skills add nvidia/skills --skill cuopt-numerical-optimization-api --agent cursor

# 更新已安装的 Skills
npx skills update

# 查看已安装
npx skills list

# 查看可用 Skills
npx skills add nvidia/skills --list
```

**支持的代理客户端**：Claude Code、Codex、Cursor、OpenClaw、Kiro、Aider、Augment 等。

## Skills 目录分类（163+ 技能）

| 分类 | 覆盖范围 | 示例技能 |
|------|---------|---------|
| **Agentic AI** | RAG、AI-Q、沙箱、策略、评估 | RAG Blueprint、AI-Q、NemoClaw |
| **Conversational AI** | 语音NIM、ASR/TTS/NMT部署 | Nemotron Speech (Riva) |
| **Data Science** | 数据准备、探索、加速分析 | cuDF、cuPyNumeric |
| **Decision Optimization** | 路由、调度、数值优化 | cuOpt |
| **GPU Development** | CUDA内核、框架集成 | TileGym |
| **Inference AI** | 模型服务、部署、运维 | Dynamo、NeMo Platform |
| **Simulation and Modeling** | 天气、气候、量子、物理ML | Earth2Studio、CUDA-Q、PhysicsNeMo |
| **Training AI** | 分布式训练、并行、CI | NeMo、Megatron-Core、DALI、Nemotron |
| **Vision AI** | 视频分析、搜索、摘要 | DeepStream、Video Search |
| **Physical AI & Omniverse** | USD资产、SimReady、实时查看 | Omniverse 工作流 |

## 各产品线技能清单

### Agentic AI
- **aiq-research** — NVIDIA AI-Q Blueprint，部署本地AI-Q服务，运行研究工作流
- **aiq-deploy** — AI-Q 部署配置

### RAG（⭐ 重点，见单独笔记）
- **rag-blueprint** — 部署、配置、排障 RAG 流水线
- **rag-eval** — RAGAS 质量评测
- **rag-perf** — 延迟/吞吐性能基准

### CUDA-Q（量子计算）
- **cudaq-guide** — 安装、测试程序、GPU仿真、QPU硬件、量子应用

### 数据科学
- **accelerated-computing-cudf** — GPU DataFrame、pandas加速、ETL、多GPU
- **cupynumeric-hdf5** / **cupynumeric-install** / **cupynumeric-migration-readiness** / **cupynumeric-parallel-data-load**

### 决策优化
- **cuopt-install** / **cuopt-multi-objective-exploration** / **cuopt-numerical-optimization-api** / **cuopt-numerical-optimization-formulation** / **cuopt-routing-api-python** / **cuopt-server-api-python**

### 训练AI
- **data-designer** — NeMo Data Designer 声明式合成数据集
- **dali-dynamic-mode** — GPU加速数据加载
- NeMo / Megatron-Core / Nemotron 相关技能

### 推理AI
- **dynamo-interconnect-check** / **dynamo-recipe-runner** / **dynamo-router-starter** / **dynamo-troubleshoot**

### 视觉AI
- **deepstream-dev** / **deepstream-generate-pipeline** / **deepstream-import-vision-model** / **deepstream-profile-pipeline**

### Physical AI
- **holohub-app-lifecycle** / **holoscan-install-*** / **holoscan-setup**

### DOCA（网络加速）
- 大量 DOCA 相关技能（部署、调试、加密、遥测等）

### 地球科学
- **earth2studio-create-datasource** / **earth2studio-deterministic-forecast** / **earth2studio-install** 等

### 数字健康
- **digital-health-clinical-asr-*** — 临床ASR评估、微调工作流

## 信任与安全机制

NVIDIA-verified Skills 经过五重保障：

1. **Cataloged** — 来自产品团队或源仓库
2. **Scanned** — 发布前扫描软件风险和代理原生风险（隐藏指令、提示注入、触发滥用等）
3. **Evaluated** — 对比有/无技能的代理输出，按正确性、安全性、可发现性、效率评估，发布 BENCHMARK.md
4. **Signed** — 用 detached OMS 签名，用户可验证下载的技能未被篡改
5. **Documented** — 附带技能卡片，记录功能、所有权、许可、依赖、限制

## 相关笔记
> 相关笔记：[NVIDIA Build NIM 免费 API](2026-08-08-nvidia-build-nim-api.md) · [NVIDIA RAG Blueprint 详解](2026-08-08-nvidia-rag-blueprint.md)
