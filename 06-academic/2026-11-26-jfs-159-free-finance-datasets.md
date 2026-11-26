# JFS 整理的 159 个免费金融与会计研究数据源：八大板块与取数清单（经管智能体）

- **日期**：2026-11-26（收录）；原文发布 2026-11-26
- **来源**：微信公众号「经管智能体」，原标题「别再花钱找数据了！159 个免费金融数据源，八大板块全覆盖！」
- **原文**：https://mp.weixin.qq.com/s/acrH3X-8Mt481xgrUh0KSA
- **清单原址**：https://www.jointfinanceseminar.com/free-datasets（*Free data sources for finance and accounting research*，已核对可访问）
- **分类**：AI 学术应用 / 科研数据资源 / 实证金融
- **相关**：[耶鲁博士课 Claude Code for Accounting Research](2026-10-29-yale-claude-code-accounting-research.md) · [徐轶青 StatsClaw 与大规模复现](2026-11-02-yiqing-xu-statsclaw-scaling-reproducibility.md)

## 清单本身

- 整理方是 **Joint Finance Seminar（JFS）** 的组织者，其中一位是伊尔梅瑙工业大学金融学助理教授 **Gerrit Köchling**（研究方向：公司金融、ESG 与信息披露，偏好文本类非结构化数据）。JFS 是 2020 年起持续运行的线上研究研讨会系列，这份索引是研讨会的配套产出。
- 页面标注 **159 sources**，组织者逐条打开检查过链接，**最近一次检查在 2026 年 11 月**；绝大多数文件可直接下载，少数需免费注册。页面留有纠错邮箱，欢迎补充失效链接。

## 先看门槛：四种"免费"不等价

| 获取方式 | 说明 | 例子 |
|---|---|---|
| 直接免费下载 | 页面默认项，无需注册 | Kenneth R. French Data Library、FRED 部分序列 |
| 免费注册 | 账号免费但要先注册 | Single-Family Loan-Level Dataset、LobbyView、World Values Survey |
| 仅限非商业学术用途 | 有用途限制，商用需另谈 | Binance 市场数据、Global Factor Premia、Econlinguistics 电话会文本 |
| 免费申请或仅在线查询 | 需申请，或只能网页检索 | Global Corporate Patent Dataset、EDGAR 全文检索、Google Trends |

页面自己也提醒：清单不完整、各来源条款不同，用之前要回提供方核对。

## 八大板块

### 1. 资产定价与市场（最密集的一块）
- **因子与组合收益**：Kenneth R. French Data Library（Fama–French 三因子、五因子及各组合收益）；AQR 数据集（betting against beta、quality minus junk、value and momentum everywhere）
- **长历史序列**：Shiller 在线数据（1871 年至今的月度股价、股息、盈利、利率、CPI、CAPE，另附美国房价）；Damodaran Online（1928 年以来美国股票、债券与国库券收益）
- **预测变量与新近因子**：Welch–Goyal 股权溢价预测变量（更新到 2025 年）；JKP 因子（Jensen–Kelly–Pedersen，93 个国家、13 类主题、153 个特征）；Hou–Xue–Zhang q 因子（1967–2025，日度到年度）
- **情绪与微观结构**：Baker–Wurgler 投资者情绪指数（到 2025 年）；Bogousslavsky 的知情交易强度、订单失衡波动、收盘集合竞价数据；Cboe 的 VIX、VVIX、OVX、GVZ

### 2. 公司金融与会计（重点是文本与治理变量）
清洗过的 **10-K 全文**、CEO 强制变更、CFO 离职及证据编码、CEO 薪酬的 **delta 与 vega**、**Loughran–McDonald 情绪词典**。省掉的是最枯燥的清洗环节，代价是要留意每条的样本区间与更新频率。

### 3. 银行、信贷与家庭
纽约联储银行资产负债表、FDIC 数据、CFPB 消费者投诉、纽约联储家庭债务面板、Fannie/Freddie 单户贷款级数据、消费者信心调查。贷款级数据多需注册，申请前先看用途限制。

### 4. 创新与年轻企业
KPS 专利价值、Ewens 组织资本、Census 企业注册数据。

### 5. 监管、政治与政策不确定性
Baker–Bloom–Davis **EPU 指数**（美国月度从 1900 年起，日度从 1985 年起，含分类子指数与多国版本）；Caldara–Iacoviello **地缘政治风险指数**；Hassan 等人的政治关联数据。

### 6. ESG、气候与不当行为
**Sautner 等人的企业气候风险暴露**（来自电话会文本，34 个国家、1 万多家公司，更新到 2024 年）；Giglio 的生物多样性风险、碳价不确定性等新议题数据。

### 7. 文化、注意力与新闻
WSJ 新闻主题注意力（Kelly 等）、Google Trends、Cookson 社交媒体注意力——行为金融常用的"注意力"代理变量，共同点是抓取自公开文本与搜索行为。

### 8. 宏观与国际
FRED 与 FRED-MD、BIS、**Jordà–Schularick–Taylor 宏观金融长历史数据库**（18 个发达经济体，1870 年起）、World Bank、IMF。JST 的价值在于把股票、住房、国债与国库券的总回报放进同一张表里。

> 页面末尾另有"其他数据源清单"附录，收纳更多第三方整理入口；"八大板块"指上述八个主题板块，附录另算。

## 按问题反查，比按类别正着翻省时间

| 想问的问题 | 优先看哪类数据 |
|---|---|
| 因子能不能解释横截面收益 | 资产定价：Fama–French、q 因子、JKP |
| 信息披露文本有没有增量信息 | 公司与会计：10-K 全文、Loughran–McDonald 词典 |
| 政策不确定性如何影响企业行为 | 监管与政治：EPU、地缘政治风险、政治关联 |
| 气候风险是否被定价 | ESG 与气候：Sautner 气候暴露、碳价不确定性 |
| 长周期里资产回报怎么变 | 宏观与国际：JST 数据库、FRED-MD |

## 拿到数据后固定三件事

1. **核对覆盖区间与更新频率**——很多看起来很新的序列实际停在几年前；
2. **确认使用条款**——尤其标注"仅限非商业学术用途"的条目，免费下载不等于可以再分发；
3. **记清原始出处**——数据集的构建方法通常写在作者论文里，引用时不能只写数据库名字。

## 可直接用的取数提示词（原文给出）

```
研究问题是【填写】。
请从以下数据源清单中筛选可用条目：https://www.jointfinanceseminar.com/free-datasets
按下面的顺序输出：
1. 哪些条目能回答这个问题，说明覆盖区间、国家/公司范围、更新频率；
2. 哪些需要注册、哪些限定非商业学术用途；
3. 每个入选条目给出原始提供方与构建方法所在的论文；
4. 不确定的地方标注"待核实"，不要推测或编造。
```

## 我的备注

- 已核对清单原址 https://www.jointfinanceseminar.com/free-datasets 可正常访问（200）。公众号文中的条目名称、作者与覆盖区间描述与原页面主题一致，但具体数字（159、覆盖年份、国家数）未逐条复核。
- 原文开头有"后台回复关键词获取 150+ Skill 下载地址"的引流钩子，与数据源清单本身无关。
- 这份清单对实证金融、会计、ESG 方向直接有用；其中 FRED、Google Trends、EDGAR、World Bank 等入口对其他实证领域同样通用。
