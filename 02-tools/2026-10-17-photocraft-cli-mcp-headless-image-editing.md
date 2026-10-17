# PhotoCraft：Rust 净室重写 Photoshop，photocraft-cli 无头修图 + 内置 MCP 让 AI Agent 直接修图（Kainy）

- **日期**：2026-10-17
- **来源**：微信公众号（Kainy，作者：明察）
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/HIiIgI_I-b5EG81xgdxAdg
- **项目**：PhotoCraft 属于 ArtCraft 开源套件（https://github.com/storytold/artcraft），背景见本库 [ArtCraft：开发者用 Claude Opus 5.5 连造 7 款开源软件对标 Adobe 全家桶](2026-10-09-artcraft-open-source-adobe-clones-opus55.md)

## 概述

PhotoCraft 是用 Rust **净室（clean-room）重新实现**的 Photoshop：不看原代码，只观察软件行为，从零写出功能相同的程序。本文重点不在"重写 PS"，而在它把图像编辑拆成 **500 多条引擎命令**，UI、命令行（photocraft-cli）和 MCP 服务器共用同一套命令注册表。界面上每个按钮、每个滑块，都能用一行命令无头完成。

## 四个核心子命令

### 1. convert：格式转换（无损回环）

```bash
photocraft-cli convert cover.png cover.psd
photocraft-cli convert photo.pcraft out.jpg --format jpg --quality 85
```

- png → 自家 `.pcraft` → png，像素完全一致，可把 `.pcraft` 当中间存档格式。
- 警告走 stderr、退出码规范，适合写进脚本。

### 2. run：无头版 Photoshop

```bash
photocraft-cli run wave.psd \
  --cmd filter.sharpen.smartSharpen \
  --cmd layer.newAdjustmentLayer.curves \
  --out wave-final.png
```

- 一串 `--cmd` 依次执行（如智能锐化 → 曲线调整层 → 自然饱和度 → 导出）。
- `--new` 可直接新建画布从零开始画。
- 相当于把 PS 的"动作面板"做成了编程接口。

### 3. batch：批量处理

```bash
photocraft-cli batch --actions grade.json --in ./raw --out ./graded
```

- 动作文件写好步骤（如自动色调 → 统一缩到 1920 宽），整目录批量处理。
- 单张失败不中断，逐个打印 ok / FAIL，最后汇总；有失败时退出码为 1，适合 CI 流水线。

### 4. mcp：内置 MCP 服务器

```bash
claude mcp add photocraft -- target/release/photocraft-cli mcp \
  --automation-read-root /work/project \
  --automation-write-root /work/project
```

- 在 Claude Code 中注册后，直接用自然语言下指令，如"打开 photos 里的 001.psd，加曲线提亮中间调，叠一层自然饱和度，导出 JPG，质量 85"。
- Agent 通过 `command_list` 自行查命令 ID 和参数文档，逐条调用。
- **`doc_render_preview`** 把编辑结果渲染成 PNG 返回给 Agent，形成"编辑 → 看图 → 再调整"的自主闭环。
- 项目带一套 10 个 Agent 修图任务的验收测试（带投影的标题卡片、智能对象 + 可调高斯模糊、转 CMYK 做印刷准备等），全程只用 MCP 工具。

## 安全设计

- 读写根目录必须显式指定，Agent 只能访问授权文件夹，越权路径直接拒绝。
- stdio 传输，不开网络端口；远程接管桌面版需走 127.0.0.1 + 256 位 token。

## 上手路径

克隆仓库 → `cargo build` → `photocraft-cli --help` 看用法 → `commands --filter` 查命令 → 用 `run` 跑通第一条管线。

## 观点

作者认为：图形界面只是套在命令外的一层糖。当年 GUI 把命令藏起来让普通人能用电脑；如今 MCP 又把 GUI 拆开，把命令重新交出去，只不过这次接命令的是 Agent——"按钮只是壳，命令才是魂"。

## 备注

- 原文未给出仓库链接，项目地址取自本库 2026-10-09 ArtCraft 笔记。
- 据该笔记，ArtCraft 官方定位 Alpha，用户试用 PhotoCraft 报告过文字选择错位、卡顿、快捷键失效等问题；CLI/MCP 用于自动化流水线前建议先小规模验证。
