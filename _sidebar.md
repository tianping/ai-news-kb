## 侧边栏

- [AI 领域进展知识库](README.md)
- [01 模型发布](01-models/)
  * [Claude 自主发现噬菌体全新酶系统 ART：2.1 亿 Token、950 Agent、21 小时](01-models/2026-09-23-claude-discovers-art-enzyme-system.md) — Anthropic 分子生物学实验室首发：Claude Agent 集群在噬菌体 DNA 中发现全新逆转录酶系统 ART，结构与 CRISPR 高度相似；Feng Zhang 审阅后称"引人入胜"；湿实验由人类完成，功能仍在验证中 (2026-09-24)
  * [Jev 作者访谈：代码才是 AI 的真正消费者，全人类陪聊天不如一个 for 循环](01-models/2026-09-25-jev-system-one-model-interview.md) — TypeSafe AI 创始人 Diogo Almeida（OpenAI RLHF 共同作者）谈 System 1 模型：RLCD 范式、模式丢弃批判、安全拒答是工程灾难、公开 benchmark 是刷榜套利 (2026-09-25)
- [02 工具与产品](02-tools/)
  * [Jev 接入 Claude Code、Codex：让 Coding Agent 学会先判断再执行](02-tools/2026-09-22-jev-claude-code-codex-integration.md) — TypeSafe AI 判断型 Agent：不做内容生成，专补执行型 Agent（Claude Code/Codex）的架构分析/逻辑判断/技术决策；新户 $5 额度，成本比同类模型低 40–400 倍、输出侧免费，GitHub Skills 一键安装
  * [Human Atlas：浏览器里把人体拆成 2234 个 3D 结构（可复用资源）](02-tools/2026-09-14-human-atlas-3d-anatomy.md) — 开源日记：ashemag/human-atlas，2234 结构/15 系统/3432 术语，BodyParts3D 4.0 数据；爆炸图按系统开关拆；搜索→点选→隔离；Node 22.13+ 本地跑或 vercel 在线版；MIT 代码+CC BY 4.0 数据，二发保留署名，非医疗用途
  * [story-to-handdrawn-video：1.8k Star 的 Agent Skill，一段文案生成手绘风竖屏视频](02-tools/2026-09-14-story-to-handdrawn-video-skill.md) — gnipbao/story-to-handdrawn-video：文案或有序手绘图→3:4竖屏静音手绘动画，一句一拍（字幕→线稿→上色）；内置20种锁定配方风格每次只用一种；翻书效果；720p预览再出1080p；安装 npx skills add ... -g；本机已装同名 skill
  * [YuE2 冲上 Trending 榜首：AI 音乐终于有了自己的工作台（AINQO 分析）](02-tools/2026-09-14-yue2-ai-music-workbench.md) — AINQO NOW 005：YuE2 把乐谱（ABC 记谱）放回生成链路，先规划旋律和弦再出声，可检查可修改；改谱=生成新完整录音（非 DAW 式精确编辑）；附 yue2-music Agent Skill，《The Last Train》9 步 14 版本；翻唱走 SheetSage2 转写零样本；WildSongBench best-of-8 榜首但有筛选预算；24GB 显存起点，代码 Apache-2.0 但权重 CC BY-NC 4.0 非商业
  * [OpenAI 关停 $200 Pro 套餐，Agnes 3.0 Flash + AgnesCode 全免费实测（新智元软文）](02-tools/2026-09-14-agnes-3-flash-agnescode-free-ai.md) — OpenAI 因 Astra 算力需求关停 ChatGPT Pro $200；Agnes 3.0 Flash 三项 Token 全 $0，AA Index v4.3 得 36 分（官方称与 DeepSeek V4 Pro 0813 max 持平）；AgnesCode 整合本地项目/Skills/MCP/CLI；AGH 计划 9 月底开源
  * [stickman-video-director：Codex Skill 跑通火柴人动画，800万播放/18W粉](02-tools/2026-09-13-stickman-video-director.md) — 文案→导演提案→6条Gemini Omni Flash提示词→逐条渲染10s拼接；无API key/MCP依赖，clone即用；三画幅+黑白高饱和强调色；帧级音频对齐/多语言配音可后期加强
  * [handraw-style：261 种手绘风格 Skill，看图选编号即出图](02-tools/2026-09-13-handraw-style-261-sketch-styles.md) — 开源日记推荐：261 种编号化手绘风格，编号管画风主题管内容，双语 Prompt，参考图限线条/笔触/材质/配色防跑偏；1000+ Star
  * [OpenClaw triage 命令：专治升级翻车、Doctor 跑不动的诊断修复工具](02-tools/2026-09-24-openclaw-triage-command.md) — configGuard: skip，config 损坏也能跑；5种模式（裸跑/指定agent/--json/--run/--update-result）；脱敏诊断包不含密钥/token/聊天记录；--run 内置修复限一轮10分钟40次调用，修完 Doctor 验证 (2026-09-24)
- [老黄手撕 Anthropic 辞职研究员：AI 安全之争与菲尔兹奖得主开研究所](03-industry/2026-09-12-ai-safety-jensen-coxon-tsimerman.md) — Coxon 辞职帖"AI 拿全人类命下赌注"、Anthropic 对齐负责人 Hubinger 跟帖十年灭绝概率>10%、菲尔兹奖得主 Tsimerman 创办 MAISI 用零知识证明给 AI 安全出题、老黄回怼"荒诞极不真实"
  * [算力这么烧钱！AI长剧《后西游记》单集成本 90 万，每分钟 2-3 万（说话不忽悠复盘）](03-industry/2026-09-14-hou-you-xi-ji-90w-per-episode-cost.md) — 单集 90 万/每分钟 2-3 万/总成本 2000 万+，算力占 1/5；成本大头是上百人美术团队逐帧修+算力，不是'零成本出片'；90 万买统一人设稳定画质的上星水准，几千块草根 AI 短剧比不了；AI 降技术门槛拉高审美门槛
  * [英伟达、Palantir 开始限制 Claude 使用：数据主权成前沿模型进企业的新门槛](03-industry/2026-09-14-nvidia-palantir-restrict-claude.md) — The Information：英伟达专有信息改用 Nemotron、Palantir 要不可撤销 ZDR、Booz Allen 网络安全项目禁用 Anthropic 商业模型；导火索是 Fable 5 默认 30 天数据留存；Anthropic 推 EFS（安全日志存客户自己的 S3/Blob/GCS）；英伟达+Palantir 主权 AI 路线；模型能力与数据控制成两条独立坐标轴
  * [1990年三毛与76岁王洛宾同居传闻，核实与三毛/荷西/王洛宾生平](03-industry/2026-09-14-sanmao-wangluobin-story-verification.md) — 自媒体文《追踪未解之谜》称1990年三毛赴新疆与76岁王洛宾同居、王洛宾拒绝发生关系后三毛回台自杀，称死因与王洛宾有关。本笔记核实三毛（1943-1991）、荷西（1955-1979潜水溺亡）、王洛宾（1913-1996）三人时间线，逐条核对自媒体文中的关键断言：三毛赴新疆时间、王洛宾年龄、半碗饭细节、喀什散心、《滚滚红尘》编剧落选、肉色丝袜上吊、王洛宾8瓶烧酒等。给出可核实与存疑部分的对照，并附王洛宾之子王海成 2025 年《新京报》专访、知乎《恋曲1990》、搜狐《三毛死因揭秘》等来源。
  * [首部AI长剧《后西游记》总成本2700万，导演给出10个颠覆性判断](03-industry/2026-09-13-hou-you-xi-ji-ai-drama-2700.md) — 30集×40min，芒果TV 2.6亿播放带动芒果超媒市值+116.5亿；100+人完成传统2000人工作量；3分钟打斗戏400-500万→2-3万；算力仅占总成本1/4~1/5；99%内容将用AIGC、话语权转移到观众
- [25位菲尔兹奖得主联名抗议AI毁数学圈](03-industry/2026-09-11-fields-medal-ai-math-protest.md) — 陶哲轩/邓煜等25位菲奖得主联名公开信，批评OpenAI/Anthropic把数学难题当Benchmark和公关素材；OpenAI称1万智能体88h破解NS方程，涉嫌利用数学家未公开草稿
- [Declaration — Math and AI（英文全文）](03-industry/2026-09-11-mathandai-declaration-full-text.md) — 联名公开信原文，AI公司解题目标与数学社区目标严重错位，呼吁紧迫应对
- [CC Switch：一个应用管 8 款 AI 编程工具的配置文件](02-tools/2026-09-10-cc-switch-ai-tool-config-manager.md) — Rust+Tauri2 开源（约12.9万Star），50+供应商预设一键切换，MCP/Prompts/Skills 统一面板+云同步；SQLite单一数据源+原子写入+自动备份；卸载后原生CLI不受影响；价值在多工具多供应商场景
- [huashu-mac-use：Agent Mac电脑操控Skill](02-tools/2026-09-07-huashu-mac-use-agent-computer-control.md) — 任何Agent装后操控Mac/Blender，三层架构（脚本→AX→坐标）优先走接口不点鼠标；"能不看图就不看图"原则；四道闸防抢焦点；每步回读验证
- [Uno Router 免费开放 GLM-5.2/5.3 系列：搜索+推理版全 $0](02-tools/2026-09-07-unorouter-free-glm5-series.md) — 注册即调，6款GLM模型（search/think/thinking/flash）输入输出缓存均$0，1M上下文，OpenAI兼容；付费裸版也极便宜
- [Codex + MiniMax H3 / Seedance 2.5：不会写分镜也能做 AI 视频](02-tools/2026-09-07-codex-minimax-h3-seedance-video-workflow.md) — 九步工作流从分析参考视频到局部修错；多分镜扩展（cuimao_reverse项目）含素材包/四格分镜/Shot State Map；第一条视频标准 Checklist
- [Kinema：AI 影像智能体，一条主题出成片](02-tools/2026-09-07-kinema-ai-cinematic-agent.md) — BladeX开源AGPL-3.0，三层架构(agent+engine+studio)贯通策划→分镜→生图→配音→成片；三种渲染模式(kenburns/dubbed/native)；成本内建(--dry-run报价+budget闸)；asset血缘自动标过期；纯CPU即可
- [GPT-6 Astra 发布后 Blender 为什么抢眼](02-tools/2026-09-07-gpt6-astra-blender-why-prominent.md) — 深度分析Astra+Blender演示为何吸睛：bpy接口让AI能"动手"+命令行循环+一体化流程；Thomas Ricouard房屋案例法线修正迭代；精细渲染vs自由漫游不同门槛；安全提醒脚本有实际执行能力
- [OpenMontage：一句话把 AI 编程助手变成视频工作室](02-tools/2026-09-08-openmontage-ai-video-studio.md) — 12条pipeline+52工具+500+agent技能，七步自动化(research→compose)；质量闸门(黑帧/音频/幻灯片风险)+成本控制；Remotion/HyperFrames/FFmpeg三路渲染；与Kinema对比
- [iptv-org/iptv：13.6万Star，不存视频、只收全球公开直播频道链接](02-tools/2026-09-09-iptv-org-iptv-global-channels.md) — 主列表index.m3u粘贴进支持直播的播放器即可；epg/database/api/awesome-iptv分工咬合；GitHub Actions持续更新；法律边界写得像范本；链接保质期不在仓库手里
- [MiniMax H3 开源34天生态全景：从3.78GB量化到Redis作者纯C实现](02-tools/2026-09-07-minimax-h3-open-source-ecosystem-34days.md) — 四挑战(加速/量化/增强/突破768p)+16芯片同日适配+antirez h3.c Mac跑通；VRAM是幌子主机内存才是瓶颈
- [03 行业动态](03-industry/)
- [SSRN 重新分析：Science"光伏让鸟变少"论文证据不足](03-industry/2026-09-07-ssrn-reanalysis-science-solar-birds.md) — 新国大+南开学者三证：观鸟活动减少≠鸟少、54.6%缺失值被赋0导致假相关、换EBD/GBIF数据库复现不出；延伸到AI时代科研质量反思——稀缺的不是数据分析而是好数据和扎实证据
- [「敲代码的时代彻底结束了！」微软18年老兵放话：怪不得Win11这么烂](03-industry/2026-09-10-microsoft-ai-coding-typing-code-over.md) — David Fowler（SignalR/NuGet）：AI包揽重复编码，开发者转向架构/需求/安全审核；纳德拉称20-30%代码由AI写；Copilot Agent闭环(建环境→改仓→测试→PR)；WinUI 3开源为AI"可读懂"；Project Zenith=Win11+本地300B大模型开发机(64GB统一内存/AMD Ryzen AI Halo)；AI写得快≠写得好，审核验证更重要
- [04 论文与技术突破](04-papers/)
- [05 事件与评论](05-events/)
- [06 AI 学术应用](06-academic/)
- [GitHub 科研 AI 工具 Star 榜（2026-09-07 更新）：10 个项目把科研交给 Agent](06-academic/2026-09-07-github-research-ai-stars-sept.md) — 8月版Star榜更新：academic-research-skills领跑46.6K，scientific-agent-skills跨100+数据库最宽；ARIS/AI-Scientist-v2新入榜；选工具三维度建议
- [科研自动化 Skill 排行榜 Top10（2026-09-09 更新）：榜单零变动](06-academic/2026-09-09-github-research-ai-stars-sept-update.md) — 与09-07版完全一致：同10项目同顺序，Star数全部持平；头部格局固化，第二梯队14K密集区（ARIS/AI-Scientist/AutoResearchClaw）下次可能换位
- [Linux 终将统治桌面端、手写代码时代终结——DHH Lex Fridman 访谈精华](03-industry/2026-09-08-dhh-lex-fridman-interview.md) — DHH：过去两个月亲手写代码为零；AI Agent 理解意图而非执行指令；一个人=过去大公司的软件生产力
- [美国拟封堵中国 AI 海外算力通道，东南亚数据中心或受审查](03-industry/2026-09-08-us-china-ai-compute-export-controls.md) — BIS 拟将远程访问限制写入出口许可证，要求第三国数据中心 KYC 审查；法律定性争议；泰国/新加坡两难
