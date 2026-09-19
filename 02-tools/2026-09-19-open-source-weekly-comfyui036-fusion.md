# 开源周报:ComfyUI 0.36 开始管音乐和视频,Nvidia 用 Rust 写 GPU 内核

## 元数据
- 日期:2026-09-19
- 来源:沉渊说(公众号周报)
- 分类:02-tools(开源项目速览,涵盖多模态生成/GPU编程/内存分配/本地搜索)
- URL:https://mp.weixin.qq.com/s/pBIJGGPfAC3PsS1BVAgXSA

## 概述
一篇开源项目速览,本期覆盖 ComfyUI 0.36、jemalloc 5.4.0、Bend 2、Nvidia CUDA Rust 官方支持、Hister 私人搜索引擎等 8 个项目。

## 关键内容

### 热门动态

**ComfyUI v0.36.0(9月15日,★13.4万,Python)**
- 接进 Yue2 音乐模型节点,生成时长可调;补 Marigold v2、Video Concatenate 节点、Bria 的 Video Eraser。
- OpenRouter partner node 塞进微软 mai-image-2.6 和 Gemini 3.8 Flash。
- Windows AMD 卡 VA 配额提到 4TB;新增「泛型循环」控制流。
- 评论:ComfyUI 越来越像"一门没有文档的编程语言";泛型循环让迭代不再靠复制粘贴节点。

**jemalloc 5.4.0(★1.1万,C)**
- 160+ 提交,新特性只有一个:EXTENT_ALLOC_FLAG_PINNED 让自定义 extent 分配钩子把 HugeTLB 等不可回收映射标出,绕过 decay/purge 优先复用,配 stats.pinned 统计接口。
- tcache 填充/保留策略改成按观察需求自适应;删除 7 个旧调节选项(lg_tcache_nslots_mul、tcache_gc_delay_bytes 等),升级后静默忽略。
- 注意:5.3.0 停在 2022 年,5.3.1 是今年 4 月,5.4.0 实际是攒了多年的账;以前调过的 tcache malloc_conf 参数可能已白调。

**Bend 2(HN 548 分,★2.1万,TypeScript)**
- 把类型检查器当证明检查器用,编译器 1 秒出结果,让 agent 每改一次代码跑一遍。编译到原生码,单核接近 C,16 核或 GPU 可跑(官网称 GPU 最快 100 倍)。无线程无锁无需写 kernel。
- 配套 LAWS.bend 相当于「带证明的 AGENTS.md」:声明绝不能被破坏的性质,AI 必须交证明,证不出来不许合并。
- 思路:AI 写的代码人不逐行读,除"我相信它"外的手段是数学;前提是 law 得写对,写 law 本身还是人在干。

**Nvidia 官方下场用 Rust 写 GPU 内核(★3492,Rust,NVlabs/cuda-oxide)** — 9月8日博客宣布两条路:
- cuda-oxide:自研 rustc 后端,SIMT 风格 kernel 纯 Rust 写,走 Pliron IR(Rust 实现的类 MLIR 框架)→LLVM→PTX,宿主和设备代码同文件,cargo oxide build 一把过。承认还是 alpha。
- cutile-rs:Tile 风格编程,跑 stable Rust 1.89+ 和 CUDA 13.3,不用自编 LLVM,线程映射和内存布局交给编译器,已上 crates.io;HuggingFace Grout 推理引擎和 mistral.rs 在用。可上生产。
- 两条路都在编译期做内存安全(cuda-oxide 用 DisjointSlice+启动契约,cutile-rs 用张量分区和所有权保证);官方称会打通 CUDA Rust 与 C++/Python 互操作。
- 意义:选择权换了人,以前靠第三方生态(cudarc、rust-cuda),现在 Nvidia 亲自下场;Nova 驱动和 Dynamo 本来就是 Rust 核心。

**Hister(Go,★4774,作者 asciimoo,searx 作者)** — 私人搜索引擎
- 不搜公开网络,索引自己的足迹:抓访问过的网页和本地文件做全文索引。三个入口:Web UI(127.0.0.1:4433)、终端、MCP server(agent 可直接翻历史)。配 Firefox/Chrome 扩展。
- v0.19.0(9月3日)加 ChatGPT 对话和 HN 讨论串提取器。
- 价值:MCP 入口让 agent 记住用户上周看过什么(本地记忆);代价是服务器需一直开着。

### 新发现

**skillbox(TypeScript,★147,作者 Kitze,昨天新建)** — 给 agent skills 建带版本的库
- 自托管 skills 库:文件/Markdown 编辑、不可变修订、冲突检测、回滚;profiles 按权限授予,客户端 key 可吊销、用量可上报、更新要 owner 审;HTTP MCP + Node/Bun stdio 桥;CLI 下载带校验和;Docker 安装。
- 意义:skills 散落在各项目 .claude/skills,靠复制粘贴复用,已从配置文件长成需版本管理的资产;「不可变修订+权限+可吊销 key」写的正是眼下靠约定硬凑的部分。

**anthropics/uplifting-biomolecular-modeling(Python,★181)** — 36 个推理加速套件
- 给一批开源蛋白质/基因组 ML 工具的推理路径做即插即用优化,一个工具一个目录;覆盖结构预测、共折叠、binder/序列设计、蛋白质和基因组语言模型。
- 每个套件旁放固定版本 upstream 原样副本(stock),优化默认不生效,靠命名 mode 打开,调用方式不变。两个 foundry 套件(rfdiffusion3、rosettafold3)铺第二解释器文件覆盖。
- 价值:敢动别人代码还留退路——固定 upstream 版本、默认不改行为、一个开关切回去。

**procedural-film(JavaScript,★122,kuhnhomeuk-cell,昨新建)** — skill 把主题变 30 秒竖屏短片
- 零媒体素材约束:每像素用原生 JS 在 canvas 画,每声音用 Web Audio 合成,最后吐自包含 HTML 播放器+MP4。参考片:帝王蝶一生,17 镜头、32 秒、120bpm、1080×1920、24fps。
- 需 Node 20+、ffmpeg、能派并行子 agent 的 agent(如 Claude Code)。
- 价值:零素材→无版权和体积问题,画面变成代码可 diff、可版本化;「agent skill+程序化生成」比套视频模型有意思。

## 影响与备注
- 本期反映的方向:多模态生成(ComfyUI)、GPU 编程成熟(Rust/Nvidia)、AI 代码验证(Bend 证明)、本地私人搜索(Hister)、skills 资产管理(skillbox)。
- ComfyUI 0.36 接 Yue2 音乐与视频节点,与 ai-news-kb 中 Yue2 AI 音乐工作台笔记可互链参照。

## 项目链接清单
- ComfyUI:https://github.com/comfyanonymous/ComfyUI
- jemalloc:https://github.com/jemalloc/jemalloc
- Bend:https://github.com/HigherOrderCO/Bend
- cuda-oxide:https://github.com/NVlabs/cuda-oxide
- Hister:https://github.com/asciimoo/hister
- skillbox:https://github.com/kitze/skillbox
- uplifting-biomolecular-modeling:https://github.com/anthropics/uplifting-biomolecular-modeling
- procedural-film:https://github.com/kuhnhomeuk-cell/procedural-film