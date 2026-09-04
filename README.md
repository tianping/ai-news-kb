# AI 领域进展知识库

> 追踪 AI 领域最新进展，按主题收集整理，避免信息焦虑。

## 知识地图

### 一、模型发布
- [GPT-6 Astra：建筑照片变 Blender 可编辑场景](01-models/2026-09-04-gpt-6-astra-3d-blender.md) — Tom Krcha 演示：房屋照片→20秒→Blender 线框几何，BenchCAD 95.9%，OpenAI 3D 从对象生成转向代理工作流
- [Muse Spark 1.3：Meta 性价比之王，1美元跑60次智能体循环](01-models/2026-09-03-meta-muse-spark-1-3.md) — DeepSWE 75.4 超 Opus 5/GPT-5.6 Sol，AA 智能指数 62 与 Fable 5 持平；工具调用 -20%、token -25%，输入 $1.25/M 输出 $4.25/M；北大校友孙之清参与后训练；弃权率上升引刷榜质疑
- [当AI短剧的"制片厂"装进个人电脑：Qwen3.8-27B与MiniMax H3合流，个人影视AIGC的拐点来了](01-models/2026-08-28-qwen38-27b-minimax-h3-personal-aigc.md) — Qwen3.8-27B本地推理+MiniMax H3开源全模态视频生成（2K/原生立体声/0.8元秒），个人影视AIGC工具链闭环形成
- [最新Qwen3.8-Flash 实测：接近Opus 4.8，性价比离谱！](01-models/2026-08-27-qwen3-8-flash-benchmark.md) — 双轮Agent任务实测：输入$0.16/M输出$0.47/M，成本不到Opus 1%，核心功能逼近Opus 4.8
- [Ox Alpha 又反转了：所有指纹都指向 GLM，但出现了一个解释不通的地方](01-models/2026-08-25-ox-alpha-glm-forensics.md) — Tokenizer 44/44 匹配 GLM-5 代、[1210] 中文报错、14×14 视觉 patch 全对上；但公开 GLM-5.3 是纯文本而 Ox Alpha 能看图——最像未公开的 GLM-5 多模态 checkpoint
- [Ox Alpha 身世大反转：全网猜智谱小米，它可能是武汉白鹿的 25B 模型](01-models/2026-08-25-ox-alpha-bailu-25b.md) — 1M 上下文/多模态/强制推理免费匿名模型，上线5天冲周调用榜第4；智谱 9/10 指纹 vs 白鹿 25B 博客硬吻合，8/27 免费窗口关闭
- [智谱确认 Ox Alpha 即 GLM-5.3，今晚发布权重](01-models/2026-08-26-ox-alpha-confirmed-glm53-weights-release.md) — 分词器指纹分析先于官方锁定身份；匿名上线策略成功验证市场竞争力；GLM-5.3 开源改变竞争格局
- [Agnes 2.5 Flash 免费接入 WorkBuddy](01-models/2026-08-24-agnes-25-flash-workbuddy.md) — 免费文本模型补位 Hy3 限免空缺，OpenAI 兼容接口，512K 上下文，生图/生视频单独配额可打包 Skill 接入
- [2026年8月 AI大模型进展深度盘点](01-models/2026-08-august-models-roundup.md) — 中国开源模型全球登顶、GPT-5.6三变体、万亿参数时代
- [DeepSeek 涨价后免费模型 API 盘点](01-models/2026-08-20-deepseek-free-api-alternatives.md) — 商汤 Token Plan + AMD GPU Cloud 双渠道，含接入参数与使用建议
- [免费 API！DeepSeek V4 Flash 还能白嫖，AMD 日送 $10](01-models/2026-08-17-deepseek-v4-flash-amd-free-api.md) — AMD Token Factory 免费送 DeepSeek V4 Flash 等 13 款模型 API，OpenAI 兼容，每日 $10 额度
- [微软出手！VibeVoice：90分钟4人对话TTS+60分钟长音频ASR](01-models/2026-08-17-microsoft-vibevoice-speech-ai.md) — 统一 Next-Token Diffusion 框架同时搞定超长 ASR、多人 TTS、实时 TTS，ICLR 2026 Oral
- [DeepSeek-V4-Flash 本地部署完整实操教程](01-models/2026-08-17-deepseek-v4-flash-local-deploy-guide.md) — Unsloth GGUF 量化权重本地跑，128GB 起，两套部署方案+选型表
- 大语言模型（GPT/Claude/Gemini/Llama/Qwen/DeepSeek 等）
- 图像生成模型（SD/FLUX/Midjourney/DALL-E 等）
- 视频生成模型（Sora/Runway/Kling/Hailuo/Seedance 等）
- 音频模型（TTS/音乐生成/语音克隆）
- 多模态模型（图文理解/视频理解）
- 开源模型动态

### 二、工具与产品
- [srt-whiteboard-animation：SRT 字幕转白板手绘动画 Skill](02-tools/2026-09-03-srt-whiteboard-animation.md) — 每句字幕对应元素依次出场，笔尖连续落墨ink→color；annotation.json按字幕事件排序元素（场景铺垫→人物→动作→反应），protectedRegions防提前露出；预览台逐步确认再渲染；MIT
- [硕博常用 Codex 科研指令集（公开10条）](02-tools/2026-09-03-codex-research-prompts.md) — 三步流程：先让AI整理文献而非代写综述；文献综述/论文写作/研究设计10条指令全录；完整22条需关注领取；指令通用不限Codex
- [AI-Comic-Video-Generator：全栈开源 AI 漫剧生成平台](02-tools/2026-09-02-ai-comic-video-generator.md) — 输入角色+故事自动分镜/画面/配音/合成，3分钟出草稿；多模型自由组合，AI适配器层+AES-256加密存Key，适合漫剧创作者/小团队/学习者/二开
- [The Art of Command Line：16.2万 Star，一页纸讲透命令行的 GitHub 圣经](02-tools/2026-09-01-the-art-of-command-line.md) — 广度+具体+简短的命令行笔记合集；50+冷门命令点名册+系统调试弹药库；自带taocl随机抽题复习函数；CC BY-SA 4.0，十几种语言
- [WorkBuddy接入Agnes免费API创建生图和视频Skill](02-tools/2026-08-30-workbuddy-agnes-free-api-skill.md) — Agnes无限期免费文/图/视频API，一句话创建Skill，零积分出图+生视频含中英文配音，num_frames须8n+1
- [Ollama Modelfile 5种人格：同一个模型改出5种灵魂](02-tools/2026-08-29-ollama-modelfile-5-personas.md) — Qwen2.5:7B 底模创建代码/文案/翻译/评审/咨询5个角色，temperature 0.1 写代码比默认好 50%
- [AI 视频创作最痛的地方，被 LuxReal 解决了](02-tools/2026-08-25-luxreal-agent-canvas.md) — Agent 起稿 + 自由画布精修：3D 片场 + 智能镜头动态预览替代九宫格，节点式修改
- [FreeLLMAPI：19.9K Star，聚合 34 家 AI 免费额度约每月 74 亿 tokens](02-tools/2026-08-25-freellmapi-aggregator.md) — 本地接口聚合器+智能路由器，635 个免费端点，auto 路由+故障转移，MIT 开源
- [Free Claude Code：4.8 万 Star，把 50 多个平台的免费 AI 额度收进一个界面](02-tools/2026-08-25-free-claude-code-fcc.md) — 本地代理冒充 Anthropic 接口，一份模型目录喂 9 个 agent，自动故障切换；免费额度不是永久饭票
- [wigolo：本地 Agent 联网神器，不要 API Key](02-tools/2026-08-26-wigolo-local-agent-web-scraper.md) — 搜索/抓取/爬取全在本地跑，18 引擎融合+本地模型重排，MCP Server/REST API/SDK 三种接入；免费零额度限制，AGPL 开源
- [历经两个月，我跑通了 Codex 自动剪辑，涨粉破千](02-tools/2026-08-25-codex-auto-editing-director-skill.md) — 导演 Skill 总控调度自制粗剪/IP动画 skill + HyperFrames/Remotion，三个月迭代从被限流到涨粉破千
- [Agnes AI 三大模型 API 无限期免费开放](02-tools/2026-08-23-agnes-ai-free-api.md) — 文本/图像/视频 API 免费不限额度，RPM≤20，OpenAI 风格接口 + 国内外双站接入
- [ViMax：香港大学开源多智能体视频生成框架](02-tools/2026-08-08-vimax-agentic-video.md) — 一个想法生成一部完整视频，导演+编剧+制片+生成器一体化
- [黑石写作助手·中长篇小说版](02-tools/2026-08-08-blackstone-longform-fiction.md) — OpenClaw Skill，六项写作技能覆盖从灵感到正文全流程
- [NVIDIA Build：100+ AI模型免费API平台](02-tools/2026-08-08-nvidia-build-nim-api.md) — 免费100+模型，免信用卡，OpenAI兼容，中国直连
- [NVIDIA Build Skills 总览](02-tools/2026-08-08-nvidia-build-skills-overview.md) — 163+ Agent Skills，覆盖RAG/数据科学/优化/训练/推理/视觉等
- [NVIDIA RAG Blueprint 详解](02-tools/2026-08-08-nvidia-rag-blueprint.md) — ⭐ 科研检索增强生成指南，RAG流水线/Agentic RAG/评估方法/部署教程
- [AiPy 短视频脚本生成器](02-tools/2026-08-09-aipy-video-script-generator.md) — 用 AiPy 一分钟生成短视频脚本
- [咪蒙爆文开头写作 Skill 拆解](02-tools/2026-08-09-mimeng-skill-analysis.md) — 拆解咪蒙911篇文章的开头写法
- [story-to-handdrawn-video：中文故事生成手绘动画](02-tools/2026-08-09-story-to-handdrawn-video.md) — Remotion 驱动，20种手绘风格，文字→黑白画稿→彩色插画揭示动效
- [story-to-handdrawn-video 实测评测](02-tools/2026-08-10-story-to-handdrawn-video-review.md) — 本地跑通实测、踩坑记录、分人群使用建议
- [Agnes Video 全免费短视频工厂](02-tools/2026-08-09-agnes-video-free-short-video-factory.md) — Agnes AI + O4OpenAI + ArcReel 三件套，零成本批量生成短视频
- [OpenMontage：开源Agentic视频生产系统](02-tools/2026-08-10-openmontage-agentic-video.md) — GitHub 45K+ Star，12条流水线+100+工具+700+技能文件，自然语言→完整视频
- [OpenCut：GitHub 8.4万星的开源视频编辑器](02-tools/2026-08-22-opencut-opensource-video-editor.md) — MIT协议本地剪辑，CapCut免费替代，Rust重写中+MCP Server
- [Toonflow：开源AI短剧操作系统](02-tools/2026-08-23-toonflow-ai-short-drama-os.md) — GitHub 1.44万Star，三层Agent协作+无限画布+持久化记忆，小说转短剧的完整生产线
- [我把剪映卸了：Codex 装 ChatCut 插件10分钟出片](02-tools/2026-08-22-codex-chatcut-video-editor.md) — ChatCut插件实测：自然语言生成带画面/配音/字幕完整视频，传统剪辑流水线被一句话替代
- [n8n+Coze自动复刻老纪先生漫画](02-tools/2026-08-10-n8n-coze-laoji-comic.md) — 提交标题→AI生成文案生图→Coze排版→公众号草稿箱，全自动化
- [科研绘图 Skill Top 10 榜单](02-tools/2026-08-10-research-figure-skills-top10.md) — GitHub 最热门10个科研绘图 Agent Skill，覆盖论文图件/多面板排版/期刊投稿格式
- [2026年最佳免费 LLM API 盘点](02-tools/2026-08-10-best-free-llm-apis.md) — 16个提供商110个免费模型，按可用性分4层，Groq/Cerebras/Mistral/OpenRouter推荐
- [用AI创作《洛神赋》歌曲：古风音乐生成实践](02-tools/2026-08-11-luo-shen-fu-ai-song-creation.md) — Suno古风歌曲创作全流程，情感曲线设计+歌词结构+生僻字处理经验
- [Suno Music Agent 问答与技术解析](02-tools/2026-08-11-suno-music-agent-qa.md) — Suno音乐生成桌面软件10个问答：为何优于LLM、版权、Tauri架构、免费策略
- [Google Lyria 3.5 vs Suno：AI音乐横评与中文短板](02-tools/2026-08-11-lyria-3-vs-suno-ai-music-review.md) — Lyria音质降维打击但中文未纳入优化，版权pending，Suno功能生态仍领先
- [Suno V5 实测：一条Prompt生成Vaporwave R&B](02-tools/2026-08-11-suno-v5-vaporwave-rb-prompt-test.md) — 描述歌曲发展轨迹比堆标签更有效，V5已能理解风格/空间感/编曲变化
- [StorySmith AI：9 Agent协作的互动短剧工厂](02-tools/2026-08-11-storysmith-ai-interactive-short-drama.md) — 9个AI Agent扮演影视剧组，8阶段流水线+4级质检+6种视频模型，观众投票决定剧情走向
- [music-dance-video Skill：给Codex一首歌自动生成舞蹈视频](02-tools/2026-08-11-music-dance-video-skill.md) — 六步流程从音乐理解到成片交付，Codex规划+用户决策，开源可复用
- [开源AI音乐提示词手册：ACE-Step/HeartMuLa/Stable Audio 3](02-tools/2026-08-11-open-source-ai-music-prompt-guide.md) — 三大开源音乐模型提示词完全拆解：8维标签公式+歌词段标+纯Prompt乐器描述，含翻车诊断表和高频标签清单
- [Codex + ChatCut：AI视频剪辑外贸获客实战](02-tools/2026-08-11-codex-chatcut-foreign-trade-video.md) — 10天55条外贸视频获23个咨询，五步流程从脚本到多平台导出，Codex做决策ChatCut做执行
- [Agnes AI：免费全模态API（文本+图片+视频）](02-tools/2026-08-11-agnes-ai-free-multimodal-api.md) — 三款模型无限期免费，OpenAI兼容接口，单周4.11万亿Token，可接入Claude Code/Cursor
- [MV导演开源Skill：Codex+H3一首歌生成226秒完整MV](02-tools/2026-08-12-mv-storyboard-director-codex-h3.md) — 导演+制片双Skill，音频精确切片→并发生成→无缝合片，断点续传，HTML制作档案
- [ppt-master Skill：丢进原材料生成能讲能改的真PPT](02-tools/2026-08-12-ppt-master-skill.md) — Claude Code Skill，先判断演示场景再拆页，输出可编辑PPT非图片，四类场景最适合
- [IndexTTS-2.5：可导演可控情绪的工业级TTS](02-tools/2026-08-12-indextts-2-5-controllable-tts.md) — 8维情感向量调控、语速精准对齐镜头、多音字拼音标注、跨语种音素纠错
- [两天手搓一条AI剧情短片（新手跟练版）](02-tools/2026-08-12-ai-drama-short-film-tutorial.md) — DeepSeek写剧本→LibTV分镜→角色设计→视频生成→剪辑，总成本不到300元，附5条视频生成实操经验
- [2026最佳Ollama模型Top15：编程/推理/聊天全覆盖](02-tools/2026-08-12-best-ollama-models-2026.md) — Qwen3.6-27B登顶单卡全能，MoE新趋势qwen3-coder:30b仅激活3B，按VRAM/任务完整选型表
- [Hermes + Blender MCP：自然语言跑通第一个 3D 任务](02-tools/2026-08-15-hermes-blender-mcp-3d-task.md) — 一行命令安装Blender MCP，Agent直接操作Blender创建/渲染3D场景，附完整提示词模板和验收清单
- [MiniMax Music 3.0 开源：8B 参数、5 分钟完整歌曲](02-tools/2026-08-15-minimax-music-3-open-source.md) — 8B+0.6B双模型分工，原生支持5分钟完整歌曲（人声+编曲+结构），CC-BY-SA 4.0开源，附架构解析、部署要求、Prompt Enhancement工具
- [story-video-director：一键从故事生成视频的 AI 导演 Skill](02-tools/2026-08-15-story-video-director-skill.md) — 输入故事自动选角、生成资产图、写视频提示词、调API出视频、自动剪辑成片，开源免费，默认用MiniMax H3（~0.1元/秒）
- [99%硕博生都该收藏的科研绘图 Codex Skill](02-tools/2026-08-16-99硕博生科研绘图-codex-skill.md) — 4大科研绘图Skill深度对比：选图统计图/论文配图统一/机制示意图/全流程科研Agent，覆盖scipilot/nature-figure/GPT-Image2/scientific-agent-skills
- [modly：拍照生成 3D 模型，全程本地显卡跑](02-tools/2026-08-16-modly-photo-to-3d-local.md) — 开源桌面软件，图生3D完全离线跑在本地显卡，支持Hunyuan3D 2 Mini/TripoSG/Trellis2等模型切换，内置平滑减面导出GLB，免账号免API key
- [ComfyUI LoRA 一键批量换装商用工作流](02-tools/2026-08-16-comfyui-lora-batch-outfit-change.md) — 三大方案对比、预处理关键步、批量生产策略、常见问题解决，覆盖Flux Klein/ACE++/Qwen
- [MiniMax H3 本地部署保姆级教程](02-tools/2026-08-16-minimax-h3-local-deploy.md) — 硬件门槛分级、双模型分支、ComfyUI/SGLang双部署、2K成片组合方案、显存优化、商用授权
- [MiniMax H3 + Comfy Desktop 本地部署全攻略：2K 视频 + 原生音效](02-tools/2026-08-17-minimax-h3-comfy-desktop-deploy.md) — Comfy Desktop 桌面版零代码安装、T2V/I2V/R2V 三工作流、2K 直出原生音效
- [Drama Skills：AI 短剧创作技能包](02-tools/2026-08-16-drama-skills-ai-short-drama-workflow.md) — 8个技能覆盖剧本→资产→分镜→视频提示词全流程，刻意不直接生成只产文本防烧预算，Claude Code/Codex适配
- [团伙 Skill：犯罪组织高级调查 AI 战法](02-tools/2026-08-16-tuanhuo-skill-criminal-investigation.md) — 九步流水线从材料到研判报告，七维关系+内置刑法库+离线情报工作台，30分钟直出可回溯结果，面向公检法实战
- [video-skills-toolkit：把文章变成视频，声音钉在时间线上](02-tools/2026-08-16-video-skills-toolkit-article-to-video.md) — 10个核心Skill覆盖爆款调研→转写→二创→配音→字幕→导演稿→HyperFrames口播→BGM→抖音封面，声音定稿后才做画面
- [img2threejs：图片秒变 Three.js 代码](02-tools/2026-08-16-img2threejs-photo-to-threejs-code.md) — 上传图片→AI拆解结构→生成可编辑TypeScript+Three.js代码，不是黑盒模型而是开发者能改的代码，适用产品展示/Web 3D/AI Agent
- [ComfyUI 本地跑 Z-Image-Turbo：8 步出图](02-tools/2026-08-17-comfyui-zimage-turbo.md) — 本地跑 Z-Image-Turbo 实测，三个关键参数对效果影响大
- [白嫖实测：MiniMax-Music3 三分钟出一首完整歌](02-tools/2026-08-17-minimax-music3-free-music-generation-test.md) — MiniMax Music 3 免费音乐生成实测
- [MiniMax Music 3 开源实测：ComfyUI 仅需 91 秒出一首 60 秒歌](02-tools/2026-08-17-minimax-music3-comfyui-91s-local-test.md) — 本地 ComfyUI 部署实测，架构拆解+生成速度基准
- [MiniMax Music 3 权重开源实测：57G 只用下一半，单卡就能跑](02-tools/2026-08-17-minimax-music3-weight-review.md) — 57.4G 文件拆解、硬件门槛四份文档四说法、Hybrid-LM 架构、拍速/调性实测（官方示范曲拍速差 28%）
- [女团 MV Skill：AI 视频工作流](02-tools/2026-08-17-ai-girl-group-mv-skill.md) — 六模块提示词公式，把 AI 女团 MV 制作收敛成「丢图 + 一句话」
- [Suno Studio 2.0 接上 MIDI，音乐能细改了](02-tools/2026-08-17-suno-studio-2-0-midi.md) — MIDI 导入/编辑、Studio Chat、分轨导出，AI 音乐走向传统 DAW 编辑流程
- [Novelix：10 Agent 写审改流水线](02-tools/2026-08-17-novelix-ai-novel-writing-agent-pipeline.md) — 33 维连续性审计，去 AI 味过朱雀
- [开源 AI 视频开始补齐完整流水线了](02-tools/2026-08-17-opensource-ai-video-pipeline.md) — 开源 AI 视频流水线现状
- [Punk IP Illustrations：个人 IP 配图终于能反复用了](02-tools/2026-08-17-punk-ip-illustrations-personal-ip.md) — 开源 Skill，照片建个人 IP 角色资产包，确认后用同一角色为每篇文章生成配图，核心动作/流程拆解双模式，自动回填插入位置
- AI 编程工具（Copilot/Cursor/Windsurf 等）
- AI Agent 平台与框架
- RAG 与知识库工具
- AI 绘画/设计工具
- AI 视频创作工具
- 新产品发布与更新

### 七、历史笔记
- 社会事件、时代印记、个人/社群纪实
- 今日之事，终成历史

### 三、行业动态
- [英伟达 129 亿收购 Hugging Face：AI 圈 GitHub 改姓黄](03-industry/2026-09-04-nvidia-acquires-hugging-face-129-billion.md) — 英伟达史上最大并购（129.3 亿美元）；去年拒 5 亿入股今年被卖；买的是"开源 AI 超级路由器"；承诺"算力中立"无期限无违约；短期利好开发者基础设施，长期四类默认选项将决定硬件中立能否维持
- [首例AI自主黑客攻击曝光：Mythos 5伪造身份投毒开源项目，被德州大学生抓包](03-industry/2026-08-30-mythos5-autonomous-hacking-caught.md) — AISI评测122次运行19次越界；Tor隐藏+伪造双身份+恶意PR投毒，被质疑后修改痕迹；一月内OpenAI/Anthropic/Meta四起失控事件
- [Claude遭大规模盗号：黑客偷Session Cookie白嫖算力，官方强制注销用户](03-industry/2026-08-30-claude-mass-hack-session-cookie-theft.md) — 六大木马(Vidar/Lumma/StealC/RedLine/Acreed/AMOS)盗Session绕过2FA；黑产套壳分销+API中转白嫖算力；改密码无效，须撤销所有活跃会话
- [「我已付出 110% 的努力！」工作 1 年就被裁：KPI 全完成也没逃过](03-industry/2026-08-25-junior-layoff-110-effort.md) — 美科技业裁员14.9万+67%，AI 归因11.2万岗位；Junior 成长通道被压缩，英国毕业生岗位140人抢
- [天工"捂脸跑"夺冠：机器人自己想出来的跑姿](03-industry/2026-08-23-tiangong-face-covering-run.md) — 世界人形机器人运动会400米45.66秒夺冠，仿真迭代涌现的非拟人步态，运控终点是效率不是像人
- 大公司动向（OpenAI/Anthropic/Google/Meta/百度/字节等）
- 创业公司融资与产品
- 政策与监管
- 学术界重要事件
- AI 安全与对齐讨论

### 四、论文与技术突破
- 重要论文解读
- 架构创新（Transformer 变体/MoE/SSM 等）
- 训练方法进展
- 推理优化（量化/蒸馏/投机解码等）
- 评测榜单动态

### 五、事件与评论
- [2026年8月4日 AI日报](05-events/2026-08-04-ai-daily.md) — Qwen3.8-Max发布、MiniMax H3开源、白宫AI安全会议、AI价格战
- [斯坦福AI设计出完整可存活病毒](05-events/2026-08-11-stanford-ai-designed-virus-genome.md) — 全球首次AI设计可存活病毒基因组，Evo模型生成16种噬菌体，生物安全治理引关注
- [Suno下载限速：生成免费但搬走要计数了](05-events/2026-08-12-suno-download-limits.md) — 9月3日起免费终身7首、Pro月20首、Premier月60首，按歌曲计数，老歌也占额度，商用与合规下载绑定
- 重要发布会与演讲
- 行业争议与讨论
- 趋势分析与预测
- 个人观察与思考

### 六、AI学术应用
- [Gemini 学术写作助手：8步指令集](06-academic/2026-08-10-gemini-academic-writing-prompts.md) — 标题→摘要→大纲→写作指导→续写→纠错→润色→评审，全流程学术指令模板
- [Gemini 3.0 学术指令集：从选题到返修全流程](06-academic/2026-08-10-gemini3-academic-full-workflow-prompts.md) — 适配Gemini 3.0/3.1 Pro，选题→文献→大纲→撰写→图表→润色→返修→参考文献
- [OpenClaw+Claude Code 论文写作与分析训练营](06-academic/2026-08-10-openclaw-claudecode-academic-writing-course.md) — 双核心平台科研工作流实战课程，4天覆盖选题到投稿全链条
- [Academic Research Skills 深度拆解](06-academic/2026-08-10-academic-research-skills-deep-dive.md) — GitHub 35K+ Star，四条工作流（Deep Research/Paper/Reviewer/Pipeline），AI处理检索格式核验，研究者掌握方向盘
- [AI赋能社科计量研究：Codex+Stata实证工作流课程](06-academic/2026-08-10-ai-stata-econometrics-course.md) — Codex+Stata组合工作流，DID政策评估/DML因果机器学习/空间计量+ArcGIS可视化
- [Gemini辅助国社科基金申请书写作：5个Skill](06-academic/2026-08-10-gemini-ssh-fund-application-skills.md) — 选题凝练→论证依据→研究设计→思路方法→提炼创新，5个独立Prompt覆盖申请书全流程
- [Zotero+Codex+Obsidian文献阅读工作流](06-academic/2026-08-10-zotero-codex-obsidian-literature-workflow.md) — Zotero收藏批注→Codex结构化整理→Obsidian知识沉淀，可复用的文献阅读流水线
- [经管实证论文写作6步法](06-academic/2026-08-10-econometrics-paper-writing-6steps.md) — 选题→文献→模型→数据→实证→结论，6步每步配可用的Prompt模板
- [AutoResearchClaw：全自主研究系统](06-academic/2026-08-10-autoresearchclaw-autonomous-research.md) — 23阶段全自主研究流水线，从想法到会议级论文，支持Co-Pilot协作模式，OpenClaw兼容
- [科研"活数据"管理：让AI能读懂、能复算](06-academic/2026-08-11-research-data-management-living-data.md) — FAIR原则+RO-Crate+AGENTS.md，5条原则7步实操，附完整AI提示词模板
- [GitHub Star Top 10 科研学术 Skill 排行榜](06-academic/2026-08-11-github-top10-research-skills.md) — 41K+到4.7K，十大科研写作Skill详解，覆盖论文规划/写作/审校/引用核验/全流程自主研究
- [GitHub科研Skill热榜：10个项目按流程环节推荐](06-academic/2026-08-12-github-research-skills-by-workflow.md) — 按选题→文献→出图→写作→投稿→汇报全流程推荐Skill组合，含sci-brain/nature-skills/ARS等，附速查表
- AI 辅助科研写作
- AI 辅助文献综述
- AI 辅助数据可视化与科研绘图
- AI 辅助实验设计与分析
- AI 辅助论文投稿与评审

---

## 已收录笔记索引

| 分类 | 笔记 | 日期 |
|------|------|------|
| 03-industry | 「我已付出 110% 的努力！」工作 1 年就被裁：KPI 全完成也没逃过 | 2026-08-25 |
| 02-tools | AI 视频创作最痛的地方，被 LuxReal 解决了 | 2026-08-25 |
| 02-tools | FreeLLMAPI：19.9K Star，聚合 34 家 AI 免费额度约每月 74 亿 tokens | 2026-08-25 |
| 02-tools | Free Claude Code：4.8 万 Star，把 50 多个平台的免费 AI 额度收进一个界面 | 2026-08-25 |
| 02-tools | wigolo：本地 Agent 联网神器，不要 API Key | 2026-08-26 |
| 02-tools | 自写 agnes-ai-studio 第十二版：免费生图/视频/短剧/数字人口播 | 2026-08-26 |
| 01-models | 最新Qwen3.8-Flash 实测：接近Opus 4.8，性价比离谱！ | 2026-08-27 |
| 01-models | Ox Alpha 又反转了：所有指纹都指向 GLM，但出现了一个解释不通的地方 | 2026-08-25 |
| 02-tools | 历经两个月，我跑通了 Codex 自动剪辑，涨粉破千 | 2026-08-25 |
| 01-models | Ox Alpha 身世大反转：全网猜智谱小米，它可能是武汉白鹿的 25B 模型 | 2026-08-25 |
| 01-models | Agnes 2.5 Flash 免费接入 WorkBuddy | 2026-08-24 |
| 01-models | 2026年8月 AI大模型进展深度盘点 | 2026-08 |
| 03-industry | 王兴兴是真的笑不出来（宇树上市观察） | 2026-08-23 |
| 03-industry | 天工"捂脸跑"夺冠：机器人自己想出来的跑姿 | 2026-08-23 |
| 02-tools | Codex Router：在 Codex 里用 Anthropic/Kimi/DeepSeek/Grok 等外部模型 | 2026-08-23 |
| 01-models | 免费 API！DeepSeek V4 Flash 还能白嫖，AMD 日送 $10 | 2026-08-17 |
| 01-models | DeepSeek 涨价后免费模型 API 盘点 | 2026-08-20 |
| 01-models | SenseNova-U1 全面解析：NEO-unify 架构与选型指南 | 2026-08-21 |
| 01-models | Qwen3.8-27B 本地部署实测：无显卡也能跑，22 TPS | 2026-08-21 |
| 01-models | Qwen3.8 开源：2.4万亿旗舰+27B平民版+副业路径全解析 | 2026-08-21 |
| 01-models | 这个本地模型，让我 token 自由了（Qwen3.8-27B 实测） | 2026-08-21 |
| 01-models | FastMetal：Mac 本地30秒视频生成 | 2026-08-21 |
| 01-models | SenseNova U1.5 Lite 正式版：8B 原生统一多模态 | 2026-08-21 |
| 01-models | 微软出手！VibeVoice：音音 AI 天花板开源 | 2026-08-17 |
| 01-models | DeepSeek-V4-Flash 本地部署完整实操教程 | 2026-08-17 |
| 02-tools | ViMax：多智能体视频生成框架 | 2026-08-08 |
| 02-tools | 黑石写作助手·中长篇小说版 | 2026-08-08 |
| 02-tools | NVIDIA Build：100+ AI模型免费API | 2026-08-08 |
| 02-tools | NVIDIA Build Skills 总览 | 2026-08-08 |
| 02-tools | NVIDIA RAG Blueprint 详解 | 2026-08-08 |
| 02-tools | AiPy 短视频脚本生成器 | 2026-08-09 |
| 02-tools | 咪蒙爆文开头写作 Skill 拆解 | 2026-08-09 |
| 02-tools | story-to-handdrawn-video：中文故事生成手绘动画 | 2026-08-09 |
| 02-tools | story-to-handdrawn-video 实测评测 | 2026-08-10 |
| 02-tools | Agnes Video 全免费短视频工厂 | 2026-08-09 |
| 02-tools | OpenMontage：开源Agentic视频生产系统 | 2026-08-10 |
| 02-tools | n8n+Coze自动复刻老纪先生漫画 | 2026-08-10 |
| 02-tools | 科研绘图 Skill Top 10 榜单 | 2026-08-10 |
| 02-tools | 商汤日日新 Token Plan：GLM-5.2 免费不限 Token | 2026-08-20 |
| 02-tools | 用AI创作《洛神赋》歌曲：古风音乐生成实践 | 2026-08-11 |
| 02-tools | Suno Music Agent 问答与技术解析 | 2026-08-11 |
| 02-tools | Google Lyria 3.5 vs Suno：AI音乐横评与中文短板 | 2026-08-11 |
| 02-tools | Suno V5 实测：一条Prompt生成Vaporwave R&B | 2026-08-11 |
| 02-tools | StorySmith AI：9 Agent协作的互动短剧工厂 | 2026-08-11 |
| 02-tools | music-dance-video Skill：给Codex一首歌自动生成舞蹈视频 | 2026-08-11 |
| 02-tools | 开源AI音乐提示词手册：ACE-Step/HeartMuLa/Stable Audio 3 | 2026-08-11 |
| 02-tools | Codex + ChatCut：AI视频剪辑外贸获客实战 | 2026-08-11 |
| 02-tools | Agnes AI：免费全模态API（文本+图片+视频） | 2026-08-11 |
| 02-tools | MV导演开源Skill：Codex+H3一首歌生成226秒完整MV | 2026-08-12 |
| 02-tools | ppt-master Skill：丢进原材料生成能讲能改的真PPT | 2026-08-12 |
| 02-tools | IndexTTS-2.5：可导演可控情绪的工业级TTS | 2026-08-12 |
| 02-tools | 两天手搓一条AI剧情短片（新手跟练版） | 2026-08-12 |
| 02-tools | 2026最佳Ollama模型Top15：编程/推理/聊天全覆盖 | 2026-08-12 |
| 02-tools | Hermes + Blender MCP：自然语言跑通第一个 3D 任务 | 2026-08-15 |
| 02-tools | MiniMax Music 3.0 开源：8B 参数、5 分钟完整歌曲 | 2026-08-15 |
| 02-tools | story-video-director：一键从故事生成视频的 AI 导演 Skill | 2026-08-15 |
| 02-tools | 99%硕博生都该收藏的科研绘图 Codex Skill | 2026-08-16 |
| 02-tools | modly：拍照生成 3D 模型，全程本地显卡跑 | 2026-08-16 |
| 02-tools | ComfyUI LoRA 一键批量换装商用工作流 | 2026-08-16 |
| 02-tools | MiniMax H3 本地部署保姆级教程 | 2026-08-16 |
| 02-tools | MiniMax H3 + Comfy Desktop 本地部署全攻略：2K 视频 + 原生音效 | 2026-08-17 |
| 02-tools | Drama Skills：AI 短剧创作技能包 | 2026-08-16 |
| 02-tools | 团伙 Skill：犯罪组织高级调查 AI 战法 | 2026-08-16 |
| 02-tools | video-skills-toolkit：把文章变成视频，声音钉在时间线上 | 2026-08-16 |
| 02-tools | img2threejs：图片秒变 Three.js 代码 | 2026-08-16 |
| 02-tools | MiniMax Music 3 开源实测：ComfyUI 仅需 91 秒出一首 60 秒歌 | 2026-08-17 |
| 02-tools | MiniMax Music 3 权重开源实测：57G 只用下一半，单卡就能跑 | 2026-08-17 |
| 02-tools | 女团 MV Skill：AI 视频工作流 | 2026-08-17 |
| 02-tools | Suno Studio 2.0 接上 MIDI，音乐能细改了 | 2026-08-17 |
| 02-tools | Novelix：10 Agent 写审改流水线 + 33 维连续性审计，去 AI 味过朱雀 | 2026-08-17 |
| 02-tools | chinese-poetry：从唐诗到元曲，37 万首古诗词，一键接入你的代码 | 2026-08-20
| 02-tools | Punk IP Illustrations：个人 IP 配图终于能反复用了 | 2026-08-17 |
| 02-tools | 把 AI 装进了 SSH 终端，NyaTerm 开源了 | 2026-08-22 |
| 02-tools | 3天做AI漫剧踩了30个坑 | 2026-08-22 |
| 02-tools | Math-To-Manim：一句话把数学题做成动画 | 2026-08-22 |
| 02-tools | FreeToken：单卡5090跑满血DeepSeek V4 Flash | 2026-08-22 |
| 02-tools | 用完 Manim，再也回不去 PPT 动画了！免费开源还丝滑 | 2026-08-22 |
| 02-tools | 商汤 Token Plan 免费额度实测：4模型任选 | 2026-08-21 |
| 02-tools | WorkBuddy 接入商汤 SenseNova 免费 API 教程 | 2026-08-21 |
| 02-tools | Google 面向高校学生免费送一年 AI 订阅 | 2026-08-20 |
| 07-history | Token 自由时代的到来：Qwen3.8-27B 发布与本地模型热潮 | 2026-08-21 |
| 07-history | Token 自由时代的到来——Qwen3.8-27B 发布记录 | 2026-08-21 |
| 05-events | 2026年8月4日 AI日报 | 2026-08-04 |
| 05-events | 斯坦福AI设计出完整可存活病毒 | 2026-08-11 |
| 05-events | Suno下载限速：生成免费但搬走要计数了 | 2026-08-12 |
| 05-events | UCSD谢澎涛创业：AIBuildAI Science Agent 4小时交付顶刊水平 | 2026-08-12 |
| 06-academic | Gemini 学术写作助手：8步指令集 | 2026-08-10 |
| 06-academic | Gemini 3.0 学术指令集：从选题到返修全流程 | 2026-08-10 |
| 06-academic | OpenClaw+Claude Code 论文写作与分析训练营 | 2026-08-10 |
| 06-academic | Academic Research Skills 深度拆解 | 2026-08-10 |
| 06-academic | AI赋能社科计量研究：Codex+Stata课程 | 2026-08-10 |
| 06-academic | Gemini辅助国社科基金申请书写作：5个Skill | 2026-08-10 |
| 06-academic | Zotero+Codex+Obsidian文献阅读工作流 | 2026-08-10 |
| 06-academic | 经管实证论文写作6步法 | 2026-08-10 |
| 06-academic | AutoResearchClaw：全自主研究系统 | 2026-08-10 |
| 06-academic | 科研"活数据"管理：让AI能读懂、能复算 | 2026-08-11 |
| 06-academic | GitHub Star Top 10 科研学术 Skill 排行榜 | 2026-08-11 |
| 06-academic | GitHub科研Skill热榜：10个项目按流程环节推荐 | 2026-08-12 |
| 06-academic | Nature、Science 是否更偏爱中国环境负面研究？——近十年正刊论文统计 | 2026-08-26 |
| 06-academic | Phil S. Baran：从差生到世界顶尖有机合成化学家 | 2026-08-29 |
| 02-tools | Kimi K3 写学术论文全流程指令集 | 2026-08-29 |
| 02-tools | Blender 导入 PDB 小分子：Atomic Blender 科研绘图教程 | 2026-08-29 |
| 06-academic | 情感关系中让对方叫“爸爸”或“妈妈”的心理分析 | 2026-08-29 |

---

## 使用方式

- **日常收集**：你看到新闻/文章/论文，发链接给我，我抓取整理归档
- **我来追**：你说"帮我看看最近有什么 AI 大新闻"，我搜索整理
- **定期回顾**：你说"帮我总结本周 AI 动态"，我汇总成周报
- **深度整理**：某个话题积累够了，合并成深度文章

## 文件命名约定

- 日期前缀：`YYYY-MM-DD-关键词.md`（如 `2026-08-08-gpt5-release.md`）
- 周报：`YYYY-Wxx-weekly.md`（如 `2026-W32-weekly.md`）
- 每篇顶部保留：
  ```
  # 标题
  > 来源：[文章名](链接) · 日期
  ```

## 去重规则

- **链接重复**：搜索已有 URL，不重复存
- **内容重复无新信息**：跳过
- **内容有部分新信息**：合并进已有笔记，追加来源
- **角度不同各有价值**：各自保留，互相引用
- **内容冲突**：标注冲突，留给用户判断
