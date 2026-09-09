# 用 Blender + Codex 本地部署，打造自己的 AI 图模生产线

> 来源：微信公众号 | 日期：2026-09-09
> 原文链接：https://mp.weixin.qq.com/s/hb1D4evk51F1k22zg6aTSw
> 关键词：#AI3D #Blender #本地部署 #AI工作流 #产品设计

## 核心思路

用 Codex CLI（OpenAI 本地终端 AI 编程助手）驱动 Blender Python API（bpy），通过自然语言代替代码操作 3D 建模。告别云端按次付费，成本接近零。

## 搭建步骤

### 第一步：安装 Codex CLI
```bash
brew install python@3.12
npm install -g @openai/codex-cli
codex login
```

**踩坑：**
- macOS 权限问题：用 nvm 管理 Node 版本，不用 sudo
- API Key：免费额度自动获取；付费账号写入 `~/.codex/config.yaml`
- 完全离线方案：用 Ollama/LM Studio 跑本地模型，Codex 支持 `--custom-api-base`

### 第二步：连接 Codex + Blender
在 `~/.codex/config.yaml` 中配置 Blender 路径：
```yaml
tools:
  blender:
    path: "/Applications/blender.app/Contents/MacOS/blender"
    python_executable: "/Applications/blender.app/Contents/Resources/4.2/python/bin/4.2/python"
```
验证：在 Codex 中输入"请用 Blender Python 打印当前版本信息"，成功返回版本即连通。

### 第三步：自然语言生成 3D 模型

**场景一：基础物体**
> "在 Blender 里创建一个半径为 1 米的球体，材质是金属铬，放在白色背景里渲染一张照片"
- Codex 自动生成 bpy 代码 → 创建球体 → Principled BSDF 材质（Metallic=1.0, Roughness=0.05）→ HDRI/纯色背景 → 相机设置 → 渲染输出（30s-1min）

**场景二：迭代修改**
> "球体太亮了，改成哑光黑色，加一个暖色灯光从侧面打过来"
- Codex 记住上下文，直接修改 `Scene.objects["Sphere"]` 属性

**场景三：复杂产品模型**
> "创建一个无线充电器的 3D 模型。底座圆形，直径10cm，厚度0.8cm，磨砂白色塑料，表面微凹充电区直径4cm，边缘倒角1mm，输出glTF"
- 用 `primitive_cylinder_add` + `BEVEL` 倒角 + 布尔运算挖凹陷 + Principled BSDF 材质 → 导出 glTF

## 关键技巧：Skill 系统加速

在 `.codex/skills/` 创建 Blender 技能文件，封装常用规则：
```yaml
name: blender-product
description: 快速生成产品类 3D 模型
rules:
  - use_principled_bsdf_for_all_materials
  - set_up_standard_lighting_3_point
  - export_as_gltf_by_default
  - add_metadata_in_blend_file
```

## 进阶玩法

### 批量参数化生成
写 Python 脚本批量生成不同配色版本，用 Codex 生成代码，只需告诉它"生成5种配色的产品图"。

### AI 生成贴图材质
Codex 调用 DALL·E/MJ API 生图 → 保存本地 → 在 Blender 创建纹理节点接入。

### 版本控制
每个渲染输出保存 `.blend` 源文件 + Git 管理，Codex 可自动保存并提交。

## 常见问题

| 问题 | 回答 |
|------|------|
| 免费额度够用吗？ | 够用。每月约10-20万行代码处理量，日常使用约15美元/月（GPT-4o），免费额度基本够 |
| 渲染速度？ | Cycles 引擎简单场景 5-30s；Eevee 实时渲染零延迟。Codex 不增加渲染时间 |
| 代码质量？ | 基础几何体/材质/灯光/相机/导出准确率 90%+；复杂布尔运算可能需要1-2轮修正 |
| 能导出到 Unity/UE5 吗？ | 可以，支持 glTF/FBX/USD，glTF 最推荐（PBR材质+动画+骨骼，Unity/UE5/Three.js 原生支持） |
| 完全离线能用吗？ | 可以。Ollama 跑 codellama:34b/llama3.1:70b + Stable Diffusion WebUI 本地生图 + Blender 全离线，M3 MacBook Pro 即可 |

## 成本 vs 云端

| 方案 | 单次成本 | 月成本估算 |
|------|---------|-----------|
| 云端 AI 3D 平台 | $0.5-1/次 | $200-2400/年 |
| Codex + Blender 本地 | ~$0.001/次 | ~$15/月（GPT-4o 免费额度内或低用量） |

产出：每天几十到上百个 3D 资产，迭代速度是传统手工建模的 5-10 倍。

## 局限性

复杂角色建模、高精度硬表面、动画绑定仍依赖手工。适合作为"快速出草模"工具，不适合最终精度需求。

---
**标签：** #AI3D #Blender #Codex #本地部署 #3D建模 #产品设计
