# Virtual Biotech：斯坦福 Science 多智能体药物研发框架，8G 显卡复现 + 病理组学番外

> 来源：「小罗碎碎念」公众号（2026-09-29），收藏于 2026-10-01
> 原文：https://mp.weixin.qq.com/s/jY3nCzbPcIDgqjq1IDgGiA
> 论文：Harrison G. Zhang, Peter Eckmann, Jiacheng Miao, Andrew B. Mahon, James Zou. *The Virtual Biotech: A multi-agent AI framework for therapeutic discovery and development.* Science. 2026-09-17（Stanford / PHD Biosciences；通讯 H.G. Zhang & James Zou）

## 系统架构（Virtual Biotech / VBT）

"虚拟生物制药公司"：AI 智能体扮演 CSO 与各科科学家，自主查数据库、写代码、做统计。四层设计：

1. **CSO 办公室**：CSO Orchestrator 只拆解任务/路由/汇总结论，不碰数据；幕僚 = Chief of Staff（情报简报）+ Scientific Reviewer（挑刺质控）
2. **四个研发部门**：Target ID / Target Safety / Modality Selection / Clinical Officers，各辖专职科学家智能体（统计遗传、功能基因组、单细胞图谱、FDA 安全官、药理学家、临床试验等），每个都带专业工具箱 + 批判本能
3. **MCP 工具层**：12 个 MCP 服务器封装十余个数据库——遗传学（GWAS/QTL/gnomAD/ClinVar/PharmGKB）、药物（ChEMBL/OpenFDA/DailyMed）、互作通路（IntAct/Reactome/STRING/GO）、临床（ClinicalTrials.gov/PubMed）、疾病（OMIM/Orphanet）、组织表达（GTEx）、单细胞（CELLxGENE Census / Tabula Sapiens v2）、功能基因组（DepMap/CRISPR/Tahoe-100M）、分子靶点（Mouse/Human Protein Atlas + 可成药性评分）
4. **数据层**：Open Targets（~80,000 靶点、1,450 万蛋白互作）、CELLxGENE（1.4 亿单细胞）、Tabula Sapiens、cBioPortal 等

可审计性：每个结论可追溯到具体工具调用与数据文件；缺数据集时工具显式报错（"失败也要留痕"）。

原版跑在 H100 + Claude Sonnet 4.5，每案例 API 费 $50–59，全量参考数据 40 GB。

## 8G 显卡（RTX 4060）零 API 复现方案

全量复现不现实 → 选 3 个可本地运行的案例 + 冒烟测试：

- **冒烟测试**：自写 `download_ot_subset.py` 只下载 13 个小数据集（613 MB），目录结构与原版一致；验证数据布局 / Parquet 加载 / MCP 错误语义
- **案例 1（临床试验标注）**：复用作者开源的 37,075 条智能体标注表（`clinical_trial_labels_reconciled.csv`，5.6 万试验），补算靶点 tau 细胞特异指数后跑统计模型
- **案例 2（肺癌 B7-H3/CD276）**：
  - TCGA 肺腺癌（cBioPortal REST API，477 名患者）：OS HR=1.62 (1.05–2.48), p=0.0281, n=243，与论文图 4F 逐位吻合；DFS HR=1.82 (0.99–3.36) vs 论文 2.06，微小偏移
  - 单细胞：CELLxGENE HTAN MSK 图谱（14.7 万细胞）→ QC → 供体×细胞类型伪批量 → PyDESeq2；单一数据集正常臂无成纤维细胞，借 Tabula Sapiens 8 个健康供体当"正常臂"；结果印证 B7-H3 上调集中于成纤维细胞（SCLC log2FC=2.13，LUAD 1.79）
  - VBT 据此提名 ADC（抗体偶联药物）策略：靶向 B7-H3⁺ 成纤维细胞 + 旁观者效应
- **案例 3（gp130 轴）**：10 基因评分（OSMR、IL6ST、LIFR…）在 4 个 GEO 队列算 AUC（对应论文图 5C）
- **tau 分析**：Tabula Sapiens（27 组织、110 万细胞），只下 1,520 个药物靶点基因列（40 GB → 697 MB），WSL 合并为 1,136,218 细胞 × 1,513 基因"瘦身"图谱；K-means 二分阈值论文 0.69（复算 0.650）；Phase I→II OR=1.245 vs 论文 1.27，主要终点 OR=1.121 vs 1.12（一致到小数第三位）；不良事件率降 42%（论文 -32%，方向一致但更强）
- 多靶点药物取 tau 最小值（"最广谱靶点决定安全面"）

## 番外：VBT + GHIST（Nature Methods 2025，H&E 切片预测空间基因表达）

- 预测 tau 与实测 tau：全基因 Spearman ρ=0.55，Top-50 SVG ρ=0.651；**预测 tau 系统性偏低**（倾向平滑细胞特异性；作者猜测主因是复现 GHIST 只训了 3 epoch）
- 二分判定（tau 最高 1/4 = 细胞特异）一致性 **71.6%** → 适合"分堆"，不宜解读绝对数值
- 虚拟图谱（7 类细胞 × 280 基因）：矩阵相关 r=0.633 (p≈1e-219)；巨噬 0.77 / T 细胞 0.75 / 成纤维 0.70 可信，**内皮 0.52 / 上皮 0.46 偏弱**（验证集各仅 47/74 细胞）
- 稀疏标记基因（VWF/PECAM1/CD14/CD68 等）tau 崩塌：实测 0.65–0.91 → 预测 0.05–0.28；喂给 VBT 须按 PCC 权重加权
- 空间可视化：ERBB2（PCC=0.70）与 PTPRC（0.55）预测空间分布对得上；**PD-L1/CD274 预测失败**（稀疏表达 + 依赖转录状态而非形态）——"病理形态能猜到细胞是谁，却猜不到每个细胞此刻表达了多少检查点"

## 资源

复现包 `VirtualBiotech-Repro`（原仓库 + 脚本 + 结果）：丢进 Codex / ZCode / WorkBuddy，"请阅读并执行 REPRODUCE.md" 即可自动复现；前置条件为已完成的 GHIST 复现。代码打包上传 MedX。
