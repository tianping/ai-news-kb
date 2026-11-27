# 用 Gemini Neon + Blender MCP 做 3D 资产：配置、分步建模与五个坑（老金独立开发）

- **日期**：2026-11-27（收藏）；原文发布 2026-11-24
- **来源**：微信公众号「老金独立开发」，原标题「用 Gemini Neon + Blender MCP 做 3D 资产」
- **URL**：https://mp.weixin.qq.com/s/uOfUORZmMR2ZPq9sVhErmA
- **分类**：02-tools（3D / Blender / MCP）
- **仓库**：https://github.com/ahujasid/mcp-for-blender （官网 https://www.mcp-for-blender.com/ ）
- **状态（2026-11-27 用 GitHub API 和 PyPI 核对）**：原名 `ahujasid/blender-mcp`，现已改名 `mcp-for-blender`；MIT，约 3.6 万 star，最近 push 2026-11-26；PyPI 包 `mcp-for-blender` 2.3.3 指向该仓库，是官方包（旧包名 `blender-mcp` 停在 2.0.0）
- **相关**：[Blender 四个 AI 插件](2026-10-23-blender-ai-plugins-loopcut-video-to-blender-skills.md)、[程序员视角的 Blender 与 Agent 自动化](2026-10-23-blender-for-programmers-agent-automation.md)、[StableGen](2026-11-27-stablegen-blender-ai-mesh-texture-rig.md)、[Blueprint](2026-11-26-blueprint-claude-code-sketch-to-blender-house.md)、[blender-skills](2026-11-18-blender-skills-94-technical-art-team.md)

> 实操教程，作者用这套流程在 Blender 5.3 里做出一座「星云双旋未来大厦」滨海建筑，全程对话，没有手动拖顶点和连材质节点。

## 三件东西怎么配合

1. **Cursor 或 Antigravity 里的 Gemini Neon**：把描述写成 Blender Python（`bpy` / `bmesh`）脚本；关键是**能看图**——渲染图传回来后，自己发现穿模、漂浮、比例跑偏、画面裁切并接着改
2. **mcp-for-blender**（原 blender-mcp）：一头接 Cursor / Antigravity，一头接 Blender 插件开的本地端口（默认 `localhost:9876`）；查场景、跑 Python、用 `look` 截图、搜素材库都走这一层
3. **本地 Blender**：建几何、算 PBR 材质、EEVEE Next 实时预览，导出 `.glb` / `.fbx` / `.usd` / `.blend`

## 安装

- 准备：Cursor 或 Antigravity（模型选 Gemini Neon）、Blender 3.0+（建议 5.x，自带 EEVEE Next）、`uv`
- **uv 走官方渠道装，不要 `pip install uv`**：macOS `brew install uv`，Windows `winget install --id=astral-sh.uv -e`；装完用 `which uvx` / `where uvx` 记下绝对路径
- 一键：`curl -LsSf https://www.mcp-for-blender.com/install.sh | sh`，或已有 uv 时 `uvx mcp-for-blender setup`
- 手动配置：Cursor 改 `~/.cursor/mcp.json`，Antigravity 改 `~/.gemini/antigravity/mcp_config.json`：

```json
{ "mcpServers": { "blender": { "command": "/opt/homebrew/bin/uvx", "args": ["mcp-for-blender"] } } }
```

- Windows 用 `"command": "cmd", "args": ["/c", "uvx", "mcp-for-blender"]` 包一层，避免读不到 PATH
- Blender 端插件：`uvx mcp-for-blender install-addon`
- 装完两边都**彻底重启**（macOS Cmd+Q）；Blender 里 Edit → Preferences → Add-ons 勾选 **Interface: MCP for Blender**，3D 视口按 N 打开右侧栏的 MCP for Blender 标签，确认服务已启动（Start MCP Server / Connect to MCP server）

## 建模方法：拆步建，每步看一次图

- **第一句指令给够四个信息**：主体造型（如扭转双曲面塔楼、金属外骨骼肋、阶梯广场配镜面水池）、材质（珠光白混凝土、拉丝香槟钛金、低铁蓝灰玻璃、暖木吊顶）、光线时段（蓝调黄昏、日落暖金侧逆光加洗墙灯）、最终用途（只出图 / 进 Three.js 网页要控面数导 `.glb` / 进游戏引擎）
- **按 Collection 分步放**，方便在 Outliner 里单独隐藏和微调
- 顺序：清默认方块 → 定渲染引擎（先 `BLENDER_EEVEE`）、天空和复用材质 → 只建主结构并立刻用 `look` 看四视图 → 比例对了再加底座、裙楼、连廊、水面、树、灯柱 → 最后调主光、天光补光、室内透光和相机（建筑常用 22–28mm 广角，加一点 `shift_y` 保持竖线垂直）
- **`look` 是最管用的工具**：作者案例里模型自己看图改掉的问题——顶视图发现台阶盖住半个倒影池、肋条顶端戳出冠环、空中花园树探出阳台（改用参数公式贴合交点和内缩坐标）、大堂太扁平（拆成暗色结构核、挑空立柱、接待岛台、发光吊顶）、灰色地面换成黄昏海湾水面
- 也可以按观感直接提意见：「树冠太像光滑的球，改成有棱角和层次的造型」「镜头往后拉，把水池完整倒影框进去」「天空压暗成蓝调时刻，拉开室内和灯带的对比」
- 导出：网页 / Three.js / AR 用 `.glb`（应用修改器、嵌入材质）；Unity / Unreal 用 `.fbx` / `.usd`；要继续改就存 `.blend` 加一张高分辨率 PNG

## 三个可直接改用的 Prompt

- **单个道具**：「在 Blender 里做一个【低多边形 / 写实 PBR】的【赛博朋克自动贩卖机】，真实比例约 2.1 米高、底部贴合 Z=0。按外壳主体 → 玻璃橱窗与内部货架 → 霓虹招牌与按键细节 → 材质与三点布光分步构建，每步用 look 多角度检查修正穿模，最后导出 vending_machine.glb。」
- **建筑场景**：「参考【扎哈·哈迪德参数化建筑 / 迪拜未来博物馆】设计语言生成未来主义滨海展馆。EEVEE；珠光白曲面外壳 + 深古铜玻璃幕墙 + 暖金灯带；前景镜面水景倒影池；黄昏蓝调布光。建完从相机视角和四视图检查构图并迭代，保存 .blend 和一张 1600×900 PNG。」
- **局部调整**：「看一下当前相机视角，改三处：加低角度暖色夕阳主光拉出明暗；金属骨架降低粗糙度带香槟金；做一个 120 帧围绕主塔平滑旋转 360° 的相机动画。」

## 五个坑

1. **`spawn uvx ENOENT`**：macOS 从程序坞启动的 GUI 应用不继承终端里的 Homebrew PATH，`command` 改成 `which uvx` 查到的完整路径
2. **Blender 卡死、MCP 超时**：macOS 切到 Cycles + Metal GPU 后首次编译 shader 很慢，这时并发多个 `look(shading="rendered")` 会堵死主线程；建模调构图阶段一直用 EEVEE，并提醒 AI 每次只调一次 `look`，定稿后再切 Cycles 出大图
3. **连不上 `localhost:9876`**：多半是装插件前 Blender 就开着、或右侧栏服务没启动；彻底重启 Blender 看面板状态。**同一时间只能连一个 AI 客户端**（Cursor / Antigravity / Claude Desktop 选一个），多开会撞端口
4. **中文界面报材质节点 KeyError**：中文版把 "Principled BSDF" 显示成「原理化 BSDF」；脚本按类型找节点（`node.type == "BSDF_PRINCIPLED"`），不要按名字硬编码
5. **视口全是灰模**：默认 Solid 视图只显示 `material.diffuse_color`，真材质走节点树；按 Z 切到 Material Preview 或 Rendered

## 还能接什么

- MCP 面板可勾选 **Poly Haven**（免费 CC0 HDRI 和 PBR 贴图）、**Sketchfab**，或接 **Tripo / Meshy / Hyper3D** 等 3D 生成服务，程序化主体和外部资产混用
- 让模型把层数、扭转角度、配色抽成脚本参数，一次跑 5 个变体挑选；加相机关键帧导出旋转展示视频

## 点评

- 方法论和本库 Blueprint / cutaway 一致：**分步建 + 每步渲染回看 + 按差异修**，区别在这里用 MCP 直连运行中的 Blender，而不是让 Agent 写离线脚本
- 五个坑都很具体，第 3 条（单客户端占端口）和第 4 条（中文界面节点名本地化）对用 Claude Desktop / Claude Code 接同一个 MCP 也适用
- 一键安装是 `curl | sh`，域名已核对是项目官网；习惯上还是先手动配 `mcp.json` 更可控
- 原文只用了 Gemini Neon，换 Claude 等能看图的模型走同一个 MCP 应该同样可行，但作者没有对比
