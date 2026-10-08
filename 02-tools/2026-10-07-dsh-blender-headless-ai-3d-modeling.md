# 不用开 Blender、不用点连接：dsh-blender 让 AI 在后台建 3D 模型

- **日期**：2026-10-07
- **来源**：微信公众号「北漂的黄黄」
- **分类**：02 工具与产品
- **URL**：https://mp.weixin.qq.com/s/3Xd8kGOiS7aGnJqpx5vGpg
- **插件**：dsh-blender（GitHub `CheshireJCat/blender`，实测 v0.2.1），宿主是 DSH（DeepSeek Harness）

## 概述

BlenderMCP 那条路要开着 Blender，还要手动点「Connect to MCP」，好处是能实时看着 AI 建模。这篇介绍另一条路：给 DSH 装 dsh-blender 插件后，AI 每次操作都会自己起一个后台 Blender 进程（`blender -b`），无人值守地完成建模、渲染和导出，最后把文件交给你验收。

作者的实测链路：一句话需求 → 50 次工具调用 → 10 分 53 秒 → 8 个交付文件（`.glb` 成品、版本化 `.blend` 源文件、4 张验收渲染图）。

## 插件能力

- **13 个工具**：`blender_status`（查环境）、`blender_python`（跑建模脚本）、`blender_preview` / `blender_render`、`blender_import` / `blender_export`、场景/物体信息查询、场景校验、分析辅助
- **30 个 Skill**：总编排 `create-3d-model`，加上建模、参考图转 3D、线框转 3D、动画等 29 个领域 Skill
- **26 个分析 Helper**：参考图拟合、线框、轮廓、多视图、UV、贴图、外观、修复、动画 QA
- **导出格式**：GLB/glTF、FBX、OBJ、STL、USD、PLY、DAE
- **安全默认**：只在会话工作目录内读写，拒绝覆盖已有文件；不需要 BlenderMCP addon，也不用开端口

## 安装（作者环境是 Windows）

**前置**：Blender ≥ 4.3、Node ≥ v20、dsh ≥ 0.1.0-rc.6、pnpm 装了就行（`npm i -g pnpm`）

1. **装插件**：`dsh plugin --profile web add dsh-blender`。npm 上有个同名的「破壁机」插件，想锁定正确的包就用 `add github:CheshireJCat/blender#v0.2.1`。不过 git 源要现场构建，可能被 pnpm 拦下，见下面的报错速查。
2. **setup**：`dsh plugin --profile web exec dsh-blender-setup`，会在插件目录建 `.venv`，装 OpenCV、NumPy、Pillow、SciPy。
3. **核对**：插件目录 `~/.dsh/profiles/web/node_modules/dsh-blender/package.json` 里要能找到 `CheshireJCat`。
4. **配 Blender 路径**（Blender 不在 PATH 时必须做）：
   - **做法 A（推荐，只影响这个插件）**：在 `~/.dsh/profiles/web/cordis.patch.yml` 末尾追加一段 `- id: blender-modeling` + `config:`，字段包括 `blenderExecutable`、`timeoutMs: 180000`、`helperTimeoutMs`、`maxOutputChars`、`restrictToWorkspace: true`、`enablePython`、`enableHelpers`、`registerSkill`、`registerModuleSkills` 等。
     - 坑：`id` 是插件 cordis.patch.yml 里的 `blender-modeling`，**不是**包名 `dsh-blender`；路径里有空格或括号要加单引号；`config:` 是**整体替换**，必须照默认值把字段写全。
     - 验证：`dsh --profile web --dump-config` 输出里出现 `patched by ...cordis.patch.yml`。
   - **做法 B（改系统 PATH）**：加完后要执行 `pm2 restart dsh --update-env`，**`--update-env` 必须带**，否则 pm2 里的进程读的还是旧环境变量。
5. **重启**：`pm2 restart dsh && pm2 save`。只刷新浏览器没用。

## 使用

在 DSH 网页里新建会话，把工作目录设成建模目录，然后直接说需求，例如：

> 使用 create-3d-model 创建一个低多边形台灯。先调用 blender_status，粗模和最终版本分别保存并渲染验收，最后导出 GLB，同时保留版本化 .blend 源文件。

流程：粗模 → 渲染验收 → 细化 → 结构校验 → 导出 → 重新导入验证。

**怎么算成功**：会话里能看到 `blender_*` 工具；`blender_status` 能返回版本号；目录里出现 `.blend`、`.png`、`.glb`。

## 报错速查

| 现象 | 原因与解决 |
|---|---|
| exec setup 报 `Command not found` | 插件没装上，回到第 1 步 |
| add github 报 `pnpm failed ...prepare script` | pnpm 默认拦构建脚本，改用 npm 源 `add dsh-blender`（预构建） |
| 重启后浏览器里没变化 | 要重启 dsh 进程，刷新浏览器没用 |
| Blender 调用失败 | 第 4 步没配或路径写错 |
| pm2 报 `node ENOENT` | 在 git-bash 里敲的，换普通 CMD |
| 第 1 步超时 | 网络问题，换热点重试 |

## dsh-blender 和 BlenderMCP 怎么选

- **dsh-blender**：不用开 Blender、不用连接，过程在后台跑，看不见；适合批量、无人值守。
- **BlenderMCP**（WorkBuddy、千问办公等）：必须开着 Blender 并点 Connect（本地端口 9876），能实时看、随时介入。
- 两者可以共存：作者在 Blender GUI 开着、9876 端口正在监听时跑后台渲染，照样一次成功。只用 DSH 的话，BlenderMCP addon 留着也没坏处，只会多几行 warning。
- 类比：Zotero MCP 是本地 23119 端口，BlenderMCP 是本地 9876 端口，都只监听本机。

一句话总结：BlenderMCP 是「你看着 AI 干活」，dsh-blender 是「AI 干完活叫你验收」。

## 相关笔记

- [Hermes + Blender 跑通第一个 3D 任务](2026-08-15-hermes-blender-mcp-3d-task.md)（BlenderMCP 可见路线）
- [Blender + Codex 本地 AI 图模生产线](2026-09-09-blender-codex-ai-3d-production.md)
