# NVIDIA RAG Blueprint 详解：科研检索增强生成指南

> 来源：[NVIDIA RAG Blueprint GitHub](https://github.com/NVIDIA-AI-Blueprints/rag) · [NVIDIA Developer Blog - Build a RAG Agent](https://developer.nvidia.com/blog/build-a-rag-agent-with-nvidia-nemotron/) · [build.nvidia.com/nvidia/build-an-enterprise-rag-pipeline](https://build.nvidia.com/nvidia/build-an-enterprise-rag-pipeline) · 2026-08-08

## 什么是 RAG

RAG（Retrieval-Augmented Generation，检索增强生成）将大语言模型的推理能力与外部知识库的实时检索结合，让 AI 回答基于你的数据而非模型训练数据，减少幻觉，确保准确性和时效性。

## NVIDIA RAG Blueprint 是什么

NVIDIA RAG Blueprint 是一个**生产级参考架构**，用于构建 RAG 流水线。基于 NIM 微服务，提供：

- 模块化、可配置、可分解的设计
- GPU 加速组件
- 多模态支持（文本、表格、图表、图片、音频）
- 预构建 UI + 开源代码
- 多种部署方式（本地 Docker / Kubernetes）

- **GitHub**：https://github.com/NVIDIA-AI-Blueprints/rag
- **在线试用**：https://build.nvidia.com/nvidia/build-an-enterprise-rag-pipeline
- **许可**：Apache License 2.0

## 三种 RAG Skills

安装方式：在 RAG Blueprint 目录下执行 `npx skills add .`

### 1. rag-blueprint（运维操作）
- **用途**：部署、配置、排障、关闭 RAG 流水线
- **REST API 使用**：/v1/generate、ingestor upload
- **示例指令**：
  - "Deploy RAG with self-hosted NIMs"
  - "Enable guardrails"
  - "Wide-net search then high-precision on my collection"

### 2. rag-eval（质量评测）⭐ 科研重点
- **用途**：RAGAS 质量基准测试
- **包含**：corpus/ + train.json 数据集和 scripts/eval/evaluate_rag.py
- **示例指令**：
  - "Run RAGAS eval on my dataset"
  - "Compare reranker on vs off"
- **科研价值**：量化评估 RAG 系统的回答质量、检索准确度、忠实度

### 3. rag-perf（性能评测）
- **用途**：延迟/吞吐基准测试
- **包含**：scripts/rag-perf（性能分析 + aiperf）
- **示例指令**：
  - "Profile retrieval bottlenecks"
  - "Run a concurrency sweep"

## 核心架构组件

### 完整 RAG 流水线

```
用户提问 → 查询处理 → 向量检索 → 重排序 → LLM生成 → 返回答案+引用
```

### 各组件说明

| 组件 | 作用 | NVIDIA 技术 |
|------|------|------------|
| **数据摄取** | 多模态文档提取（文本/表格/图表/音频） | NeMo Retriever Extraction |
| **Embedding 模型** | 文档和查询转向量 | llama-nemotron-embed-1b-v2 |
| **向量数据库** | 存储和检索嵌入向量 | cuVS 加速（默认 Elasticsearch，可选 Milvus） |
| **重排序模型** | 对检索结果重新排序提精度 | llama-nemotron-rerank-1b-v2 |
| **LLM 推理** | 基于检索内容生成回答 | Nemotron-3-Super-120B 等 |
| **编排层** | 协调用户、检索器、向量库、推理模型 | LangChain |
| **护栏** | 内容安全、主题控制 | NemoGuard Content Safety / Topic Control |
| **UI** | 端到端问答工作流展示 | rag-frontend |

### 可选增强组件
- **OCR NIM**：文档光学字符识别
- **多模态嵌入**：llama-nemotron-embed-vl-1b-v2（图文混合嵌入）
- **多模态重排序**：llama-nemotron-rerank-vl-1b-v2
- **Parse NIM**：文档解析
- **ASR NIM**：语音转文字（Riva）
- **内容安全**：Llama 3.1 NemoGuard 8B
- **主题控制**：Llama 3.1 NemoGuard 8B Topic Control

## Agentic RAG（智能体 RAG）

传统 RAG 是线性的：检索→生成。Agentic RAG 使用 **ReAct 代理架构**，让 LLM 自主决定：

- **是否需要检索**：简单问题直接回答，复杂问题才触发检索
- **如何检索**：选择数据源、决定检索策略
- **多跳推理**：复杂问题拆分为子问题，并行检索，综合答案
- **验证**：可选的答案验证步骤

### LangGraph 计划-执行管线
1. **范围发现**：分析问题需要哪些信息源
2. **并行子任务**：拆分为多个检索子任务并行执行
3. **综合**：合并各子任务结果
4. **可选验证**：检查答案是否有充分依据
5. **流式阶段事件**：UI 和 API 实时展示推理过程

启用方式：
```python
# 请求级别
{"agentic": true, ...}
# 或部署级别
ENABLE_AGENTIC_RAG=true
```

## 科研场景应用建议

### 文献检索与问答
- 摄取论文 PDF（支持多模态：文本+表格+图表）
- 向量检索找相关段落，重排序提精度
- LLM 基于检索内容回答，附带引用

### 文献综述辅助
- Agentic RAG 模式处理多跳问题
- 跨文档查询：拆分子问题→并行检索→综合
- RAGAS 评估回答质量

### 实验数据分析
- 摄取实验报告、数据表格
- 自然语言查询实验结果
- 多轮对话追问细节

### 搭建步骤（科研场景）

```bash
# 1. 克隆 RAG Blueprint
git clone https://github.com/NVIDIA-AI-Blueprints/rag.git
cd rag

# 2. 安装 RAG Skills 到你的 AI 代理
npx skills add .

# 3. 配置 API Key（NGC API Key）
# 访问 https://org.ngc.nvidia.com/setup/api-key

# 4. Docker 部署（单节点）
# 参考 docs/deploy-docker-self-hosted.md

# 5. 摄取你的文献/数据
# 通过 UI 或 API 上传文档

# 6. 提问
# 通过 UI 或 POST /v1/generate
```

### Python 代码示例（Using NIM API）

```python
from langchain_nvidia_ai_endpoints import ChatNVIDIA, NVIDIAEmbeddings
from langchain_community.vectorstores import FAISS
from langchain.text_splitter import RecursiveCharacterTextSplitter

# 1. 加载模型
llm = ChatNVIDIA(model="nvidia/nemotron-3-super-120b-a12b", temperature=0.6)
embeddings = NVIDIAEmbeddings(model="nvidia/llama-nemotron-embed-1b-v2")

# 2. 摄取文档
from langchain_community.document_loaders import DirectoryLoader
loader = DirectoryLoader("./papers", glob="**/*.pdf", show_progress=True)
docs = loader.load()

# 3. 文本分割
splitter = RecursiveCharacterTextSplitter(chunk_size=800, chunk_overlap=120)
chunks = splitter.split_documents(docs)

# 4. 向量化并存入数据库
vectordb = FAISS.from_documents(chunks, embeddings)

# 5. 检索 + 生成
retriever = vectordb.as_retriever(search_kwargs={"k": 5})
from langchain.chains import RetrievalQA
qa = RetrievalQA.from_chain_type(llm=llm, retriever=retriever, return_source_documents=True)
result = qa({"query": "这篇论文的主要创新点是什么？"})
print(result['result'])
print("来源文档：", [doc.metadata for doc in result['source_documents']])
```

## 关键参数调优

| 参数 | 建议 | 说明 |
|------|------|------|
| chunk_size | 800 | 太大含无关信息，太小缺上下文 |
| chunk_overlap | 120 | 保证跨块连续性 |
| 检索 top_k | 5-20 | 广搜用大值，精搜配合重排序用小值 |
| 重排序 | 开启 | 显著提升精度 |
| temperature | 0.6 | 事实性问答用低温度 |
| guardrails | 科研可关闭 | 除非有合规需求 |

## 部署方式

| 方式 | 适用场景 |
|------|---------|
| Docker Compose（本地+Hosted NIM） | 快速试用，用 NVIDIA 托管 API |
| Docker Compose（本地+自托管 NIM） | 完全本地部署，需要 GPU |
| Kubernetes/Helm | 生产环境 |
| Red Hat OpenShift | 企业环境 |
| Python library mode | 嵌入现有应用 |

## 评估方法（RAGAS）

RAG Blueprint 内置 RAGAS 评估框架，衡量：
- **Faithfulness（忠实度）**：回答是否基于检索到的文档
- **Answer Relevancy（回答相关性）**：回答是否切题
- **Context Precision（上下文精度）**：检索到的内容是否相关
- **Context Recall（上下文召回）**：相关信息是否都被检索到

```bash
# 运行评估
python scripts/eval/evaluate_rag.py --corpus data/corpus/ --train data/train.json
```

## 相关笔记
> 相关笔记：[NVIDIA Build NIM 免费 API](2026-08-08-nvidia-build-nim-api.md) · [NVIDIA Build Skills 总览](2026-08-08-nvidia-build-skills-overview.md)
