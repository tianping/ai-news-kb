# StableGen：Blender 免费插件，一句话生成网格、贴图、骨骼（晚风逐云开）

- **日期**：2026-11-27（收藏）；原文发布 2026-11-26
- **来源**：微信公众号「晚风逐云开」，原标题「Blender 免费插件 StableGen：一句话让 AI 建模、贴图、绑骨骼全搞定」
- **URL**：https://mp.weixin.qq.com/s/aujTpEKqvgYsU6IdFvjXDQ
- **仓库**：https://github.com/sakalond/StableGen （文档在仓库 Wiki）
- **分类**：02-tools（3D / Blender）
- **状态（2026-11-27 用 GitHub API 和 README 核对）**：GPL-3.0，约 1,739 star，2025-05-15 建仓，最近 push 2026-11-04
- **相关**：[Blueprint 草图转 Blender 场景](2026-11-26-blueprint-claude-code-sketch-to-blender-house.md)、[blender-skills 94 个技能](2026-11-18-blender-skills-94-technical-art-team.md)、[Blender 四个 AI 插件](2026-10-23-blender-ai-plugins-loopcut-video-to-blender-skills.md)

> 原文称「上线不久已经 1400+ star」，不准确：项目 2025 年 5 月就建仓了，是持续迭代一年半的老项目，最近才加上网格生成和自动绑骨。原文是功能介绍，作者没有实测。

## 一句话

在 Blender 面板里写一句角色描述，从几张预览图里挑一张，StableGen 一次跑完：**TRELLIS.2 建网格 → SkinTokens 绑骨骼加蒙皮权重 → 四周多视角生成图投影成贴图**。推理都在 ComfyUI 后端，Blender 里只管操作。

## 流水线（README 原文）

- **网格**：微软 TRELLIS.2（走 ComfyUI 自带的 TRELLIS.2 节点）；也可以从图片开始，或者直接保留 TRELLIS.2 自己生成的贴图
- **绑骨**：VAST 的 SkinTokens，同一次运行里勾选 **Auto-rig** 即可；也能给任意选中的已有网格绑骨
- **贴图**：SDXL 或 Qwen Image 2.1 生成多视角图后投影
- **PBR**：Marigold + StableDelight 从生成的视图估算 albedo / roughness / metallic / normal，烘焙成标准贴图
- **后处理**：LATO.2 可以把任意网格重建成低多边形拓扑；**Make Solid** 去掉 TRELLIS.2 网格自带的内部隐藏壳

## 贴图为什么能接得上（原文重点）

- 在模型周围摆一圈相机，逐个方向生成视图再投影到表面；**每画一个新方向都参考已经画好的方向**，所以转到背面时颜色、光照、纹样接得上，不会出现一圈转回来对不上的接缝
- 相当于把画师画三视图设定稿的做法交给 AI，用「每一步参考上一步」保持一致
- 可以只给已有模型补贴图、不动网格；也可以涂选一块局部重画，其余部分不变

## 输出

- 烘焙成 UV 贴图（含 PBR），连同骨骼和动画一起导出：**glTF** 给 Godot 和网页，**FBX** 给 Unreal / Unity
- 动作：FBX / BVH 动作（如 Mixamo 动画）可以重定向到生成的骨骼上
- 还能导出环绕展示的 GIF / MP4、**多色 3MF** 用于 3D 打印；有 Scene Queue 可以无人值守批量跑
- README 示例：一个 Unreal Engine 5.7 村庄场景，房屋、道具、树和地面全是 StableGen 导出的 GLB，另有 5 个带行走动画的绑骨角色（FBX）

## 环境要求

- **Blender 4.2–4.5 或 5.1 以上，5.0 不支持**
- ComfyUI：插件可以自己装一个（约 5 GB，自带 Python 和 torch，自动启停，**目前只在 Windows 上测过**），也可以用你已有的，或连接另一台机器上的
- 推荐 NVIDIA 显卡：SDXL 约 8 GB 显存，Qwen Image 要 16 GB 以上
- 模型磁盘：按装的模型不同，需要 10 GB 到 50 GB 以上
- 安装：从 Releases 下载对应系统的 ZIP（Windows x64 / Linux x64 / Apple Silicon Mac）→ Blender **Edit > Preferences > Get Extensions > Install from Disk**；模型在插件设置页的 **Models** 里按需安装，下载中断可以续传
- 命令行装法：`python installer.py`，按编号选包（3 = SDXL 贴图，13 = Qwen Image 2.1，8 = 网格生成，14 = 自动绑骨，18 = LATO.2）；`python installer.py --create-comfyui <folder>` 装自带 ComfyUI

## 许可证注意（原文没提，README 写明）

- 插件本身 GPL-3.0，但它调用的三类模型**仅限非商用**：
  - Qwen Image 2.1 / 2.1 Turbo：Qwen Research License（Qwen Image Edit 和 SDXL 不受影响）
  - Bria FIBO 2.0：CC BY-NC 4.0，商用要和 Bria 签协议
  - TRELLIS.2 用到的图像编码器 Meta DINOv3 有自己的许可证
- 想把产出用于商业游戏或接单：贴图用 SDXL 路线，网格部分先看 DINOv3 许可

## 点评

- 价值在「一条流水线出能进引擎的资产」：网格、PBR 贴图、骨骼、动画重定向、导出全在 Blender 里完成，比单独的文生 3D 工具多走了绑骨和导出这两步
- 显存门槛和 ComfyUI 部署是新手的主要障碍；自带 ComfyUI 一键安装目前只在 Windows 上验证过
- 原文把「外包四位数」和「8 GB 显卡无限出稿」直接对比，有夸大：TRELLIS.2 网格通常还要减面、修拓扑，AI 绑骨对非人形角色也未必可靠，离直接可用还差人工检查这一步
- 和本库已有的 Blender + AI 方案互补：[Blueprint](2026-11-26-blueprint-claude-code-sketch-to-blender-house.md) / [blender-skills](2026-11-18-blender-skills-94-technical-art-team.md) 是让 Agent 写代码建模，StableGen 是用生成模型直接出网格和贴图
