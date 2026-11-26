# Claude + Blender 做 AI 视频：先用 3D 白模锁死镜头，再交给 Seedance 2.5 上色渲染（老刘的笔记本）

- **日期**：2026-11-25（收录）；原文发布 2026-11-24
- **来源**：微信公众号「老刘的笔记本」，原标题「Claude + Blender 是制作 AI 视频的最佳方式（完整课程）」
- **原文**：https://mp.weixin.qq.com/s/eTbn3ZW95u49X8br6FXTMA
- **分类**：工具与产品 / AI 视频 / Blender / MCP
- **相关**：[做一部 AI 漫剧的五步流程](2026-11-25-ai-comic-drama-five-step-workflow.md) · [Toonflow（3D 白模预演）](2026-11-25-toonflow-open-source-ai-short-drama-platform.md) · [Scenario for Blender](2026-10-23-scenario-for-blender-ai-plugin-mcp.md) · [blender-skills 94 个技能](2026-11-18-blender-skills-94-technical-art-team.md) · [Codex + Blender vs Tripo](2026-10-23-codex-blender-vs-tripo-ip-character.md)

> 原文是第一人称教程（作者和搭档角色 Bruiser 的爬行动物馆奇幻短片），看口吻像是海外 YouTube 教程的编译稿；作者自用 Higgsfield 付费订阅，多处带推广性质。

## 一句话

纯文字提示词抽卡做不出跨镜头空间一致和复杂运镜。思路是分工：**Blender 管骨架、构图和运镜（低多边形白模 + 参考视频），AI 视频模型管画质、光影、材质和声音**。再加三类锚点：3D 参考视频定动作轨迹、2K 重绘剧照定视觉质感、干声样本定音色。

## 为什么用 Blender 做参考

1. **空间一致**：房间、门、桌、光源都是固定的 3D 坐标，机位怎么转都不会"前一秒门在左、后一秒墙没了"。
2. **运镜和走位精确**：文字很难描述三维运动轨迹，一段参考视频能直接告诉模型怎么推拉摇移、角色怎么走。作者称过去要抽十几次的镜头，现在一两次就能用。

## 入门版：Blender 侧边栏里对话（Higgsfield 插件）

**安装**
1. 装 Blender（免费）。
2. 下载 Higgsfield Blender 插件 .zip，**不解压**，直接拖进 Blender 窗口，确认并登录（需要 Higgsfield 付费订阅）。
3. 按 N 调出右侧边栏（原文键名丢失，Blender 默认是 N）→ Higgsfield 标签 → MCP → **Enable MCP Control**。

**生成第一个场景**
- 侧边栏有免费模式；作者推荐切到 **Claude Opus 5.5** 效果最稳。
- 示例：疲惫男生下班 POV 推门、妻子抱怨晚归、他走回房间开灯坐下打游戏。
- **铁律：提示词必须写 low poly（低多边形）风格**，否则 AI 往场景里塞高精度细节，轻则卡死，重则爆显存/内存。只要骨架参考。
- 等 5–10 分钟（实测约 5–6 分钟）。粗糙、轻微穿模不要紧，只是给视频模型看的运动参考。
- 想加戏不用手改模型：进 **Scene Builder** 打字，例如"妻子怀里抱个哭闹的婴儿前后摇晃"，几分钟后自动改好。

**4 个必会视图操作**
| 操作 | 方法 |
|---|---|
| 缩放 | 滚动鼠标滚轮 |
| 旋转 | 按住中键拖动 |
| 平移 | Shift + 中键拖动（原文键名丢失，按 Blender 默认补） |
| 回到摄像机视角 | 小键盘 0，或 View → Cameras → Active Camera |

**两个保命习惯**
- 先 File → Save As 存档。
- 导出参考视频：Output 面板选路径 → 格式 MP4、编码 H.264 → Render → Render Animation。渲染版光影比视口预览自然，参考质量更高。

## 视觉锚点：防人物和场景崩

白模视频只有轮廓、走位、运镜，没有脸、服装、材质。直接丢给 Seedance 2.5，模型会自己瞎猜。要提前备好 **Visual Anchors**，公寓案例里三张：
- **角色设定图**：妻子正侧面长相、发型、穿搭；
- **走廊场景锚点图**；
- **电脑桌场景锚点图**。

**场景图要从 Blender 构图反推**，否则凭空生成的房间和 3D 里的门窗位置对不上：
1. Blender 里切到走廊和电脑桌视角，各截一张；
2. 截图作为参考图 1 传进 Higgsfield 生图，模型 **Nano Banana Pro，2K**；
3. 提示词：把参考图 1 改成写实、电影感的真实公寓走廊（或电竞房）剧照。

> 3D 视频定动态轨迹，2K 截图重绘定视觉质感。

## 用 Skill 写视频提示词，生成前三项核查

- 给 Claude/ChatGPT 装一个 **Seedance 2.5 Prompter** 技能：Settings → Customize → Skills → Add → Upload Skill，选 .zip。
- 斜杠调用技能，写剧情想法，上传 4 份素材（Blender 视频 + 妻子 + 走廊 + 电脑房），**每份素材都要标明身份**。
- 输出的提示词通读后，复制到 Higgsfield 视频界面选 Seedance 2.5。

**点生成前防烧积分核查**
1. **参考编号和上传顺序一致**：作者差点翻车，提示词写"图 1 是妻子"，实际妻子传成了图 3。
2. **时长和 Blender 参考视频一致**（公寓案例 25 秒）。
3. **先跑 Seedance 2.5 Draft 草稿档验货**，满意再放大到 1080p。

成片按第一人称路径推进，人物和房间还原锚点图，Seedance 2.5 还自动配了同步对白和婴儿哭声。

## 进阶版：Claude（+ Claude Code）通过 MCP 直连 Blender

和入门版的两点区别：
- **控制中枢**：Claude / Claude Code 从外部经 MCP 操控 Blender，Higgsfield 只负责生产图像和音视频素材；
- **顺序倒过来**：先出核心场景概念图，再让 Claude 看图在 Blender 里反向建 3D 空间。作者用 4 个图像模型横向对比，**Seedream 5.0** 质感、景深、氛围最好，定为主场景基准图。

**接入 Blender MCP（五步）**
1. Claude 设置 → Customize → **Connectors**，搜 Blender 安装；
2. 别以为装完就结束：View Details → **Instructions** 进官方配置页；
3. 下载 Blender MCP 插件 .zip，不解压，拖进 Blender。**官方说明要求拖放安装两次**；
4. Edit → Preferences → Add-ons 确认出现 MCP 插件；
5. System → Network 勾选 **Allow Online Access**；回 Add-ons 激活插件，点 **Start MCP Bridge Server**。

自检：在 Claude 里问 "Am I connected to Blender?"。最常见的卡点是漏开 Allow Online Access，或忘了启动桥接服务。

## 3D 阶段做减法 + 导演铁律

- 第一版生态箱空间太小、灌木杂物太满挡镜头 → 对话让 Claude 拉大场地、清掉杂物，得到 **barebone 极简骨架**。
- **不要在 Blender 里死磕树叶和灌木**：精细材质和环境细节会在视频生成阶段自动长出来，3D 只保留准确比例和骨架。
- **铁律一**：创意主导权不能外包给 Claude。它是执行副导演，不是总导演，剧情和画面构想要你自己先想清楚。
- **铁律二**：机位想不清楚就先画粗分镜（Storyboard）。
- 喂给 Claude Code 4 份素材：分镜草图 + 自己参考图 + Bruiser 参考图 + 巨型爬行动物参考图，再用大白话讲动作。巨兽在 Blender 里只是一个往外喷长舌头的几何方块，但机位节奏和空间压迫感已经立住了。
- 没有"神级提示词模板"，像开剧组会一样递参考、聊需求就行。

## 10 秒追逐戏：画面 + 运镜 + 音色三重锁定

- 导出 10 秒 Blender 动画，连同视觉素材传进 Seedance 2.5。
- 提示词核心约束：**严格锁死参考视频 1 的机位、构图和一镜到底运镜**，即高空俯拍（overhead opening）→ 俯冲拉近（descent）→ 横摇到 Bruiser（pan）→ 上仰揭示身后的巨型爬虫（tilt）。这种四段复合长镜头，纯文字抽一百次都未必成。
- **双角色音色锚点**：另外上传自己和 Bruiser 的干声样本各一段，跨镜头音色稳定，不会每句像换了配音演员。
- 时长对齐到 10 秒，生成。

> 先在 Blender 花几分钟确认镜头，再去视频平台花积分渲染。构图在花钱之前就已经定了。

## 我的备注

- 整条链路依赖 **Higgsfield 付费订阅**（插件、生图、Seedance 2.5 都在它上面），原文没写成本，Seedance 2.5 Prompter 技能也没给下载来源。
- 方法论本身工具无关，可迁移：**白模参考视频 + 从白模截图重绘的场景锚点 + 角色设定图 + 音色样本**，任何支持参考视频/多参考图的视频模型都能这样用；进阶版的 Blender MCP 换成 [Scenario 本地 MCP](2026-10-23-scenario-for-blender-ai-plugin-mcp.md) 或 headless `blender --background --python` 也行。
- 原文部分行内键名被吞（N、Shift、小键盘 0），笔记里已按 Blender 默认键位补全。
