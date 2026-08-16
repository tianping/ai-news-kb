# img2threejs：图片秒变 Three.js 代码，前端轻松做 3D

> 来源：[GitHub 爆火新项目：图片秒变 Three.js，前端终于能轻松做 3D 了](https://mp.weixin.qq.com/s/giQoGnGdFRK9nVpU1-llMw) · 2026-08-16 · 公众号「前端Hardy」

## 核心简介

img2threejs 是一个 GitHub 开源项目，核心能力：**上传一张图片 → AI 分析物体结构 → 生成可编辑的 TypeScript + Three.js 代码 → 浏览器直接运行**。

与传统 AI 3D 工具输出 .obj/.glb/.fbx 黑盒模型不同，它输出的是开发者能看懂、能修改的代码。

- **项目地址**：https://github.com/img2threejs/img2threejs
- **在线体验**：https://img2threejs.org/

## 解决的痛点

前端做 3D 的最大门槛不是 Three.js API，而是**没有模型**。传统流程：设计师建模 → 导出 glTF/GLB → Three.js 加载 → 调整材质动画，慢、贵、高度依赖专业人员。

img2threejs 把流程变成：图片 → 分析结构 → 拆分组件 → 生成 Three.js 场景代码 → 浏览器运行。

## 工作原理

分三个阶段：

1. **理解图片**：AI 分析图片内容，拆解出组件（如台灯 → 灯罩、灯杆、底座、按钮），识别材质、颜色、比例
2. **规划组件**：生成结构树（Lamp → Shade / Arm / Base）
3. **生成 Three.js**：输出 TypeScript + Three.js + Scene Graph，形成可继续开发的 3D 场景

## 代码式 3D 的价值

生成的代码类似：

```typescript
const lamp = new THREE.Group();
const body = new THREE.Mesh(geometry, material);
lamp.add(body);
```

开发者可以直接修改颜色、比例、动画、交互、接入业务逻辑。

**对比传统 AI 3D 工具：**

| 维度 | 传统 3D AI 工具 | img2threejs |
|------|----------------|-------------|
| 输入 | 建模文件 | 图片 |
| 输出 | Mesh 模型 | Three.js 代码 |
| 是否可编辑 | 较难 | 容易 |
| 是否适合前端 | 一般 | 非常适合 |
| 二次开发便利度 | 低 | 高 |

## 适用场景

1. **产品展示网站**：汽车、电子产品、家具、机械设备，上传图片生成交互模型
2. **Web 3D 页面**：官网首页、活动页面、数字展厅、游戏宣传页
3. **AI Agent + Three.js**：最有想象力的方向——告诉 AI"做一个未来科技风格的机器人展示页面"，AI 自动生成模型、代码、交互

## 意义

Three.js 让 Web 3D 变简单了，但最大门槛一直是内容生产。img2threejs 没有替代 Three.js，而是解决"我有想法但没有 3D 素材"的问题。未来 3D 开发可能变成：一句需求 → AI 生成资产 → 生成代码 → 前端修改 → 上线。
