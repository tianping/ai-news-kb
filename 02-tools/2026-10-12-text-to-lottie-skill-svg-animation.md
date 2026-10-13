# text-to-lottie：把 SVG 交给 AI，几分钟生成 Lottie 动画

- **收藏日期**：2026-10-12
- **来源**：微信公众号「极客小灶」
- **分类**：02-tools / AI Agent Skills / 前端动画
- **URL**：https://mp.weixin.qq.com/s/UA6GbB1CXiwvu3Cmdr65iA
- **项目地址**：https://github.com/diffusionstudio/lottie（skill 名：text-to-lottie）

## 概述

Lottie 是常见的前端动画格式：设计师在 AE 里做完动画，导出 JSON 和资源文件，前端用 lottie-web 或原生 SDK 播放，不用手写动画代码。diffusionstudio/lottie 仓库里的 **text-to-lottie** 技能可以根据参考画面（SVG）加文字描述，让 AI（Claude Code / Codex）直接生成可播放的 Lottie 动画文件，绕开 AE。

## 安装

- 命令行：`npx skills add diffusionstudio/lottie`
- 或在 CC Switch 的 Skills 页面搜 `text-to-lottie`，启用到 Claude Code 和 Codex

## 实测一：现成 SVG + 文字描述

素材用的是 SF Hello 的 `hello-en.svg`（https://github.com/JaceThings/SF-Hello/blob/main/SVG/hello-en.svg），目标是手写效果：沿原笔画逐渐出现。提示词大致如下：

> 根据 hello-en.svg 中的 SVG 路径制作一个 Lottie 动画。1. 让路径沿自然走向逐渐显现。2. 给路径加上 Apple 风格的渐变。3. 使用 ease-in-out 缓动和透明背景，保留原始 SVG 的形状。

- 输出：在当前目录生成 `sf-hello-lottie` 项目，并自动在 `http://localhost:3030` 起一个预览服务
- 动画文件：`sf-hello-lottie/public/projects/sf-hello/scene-1/lottie.json`（同目录还有用到的 SVG）

## 实测二：只给文字描述

> 制作一个内容为"wonderful"的 Lottie 文字动画。1. 文字笔画从左到右逐渐显现。2. Apple 风格渐变。3. ease-in-out 缓动和透明背景。

- 不给 SVG 也能生成，工具会先自己生成一个 SVG，再做动画

## 结论

- 给现成 SVG：更容易控制字形和轮廓
- 只给文字：省掉准备素材的步骤
- 对画面有明确想法时，先提供参考图或 SVG，再让 AI 做动画

## 备注

- 适用场景：网页和 App 的加载动画、Logo 动画、文字标题动效，也可以用在 PPT 或公众号动图上（Lottie 可以再导出成 GIF 或 MP4）
- 本机的 Claude Code 也能装这个 skill 来用（npx 缓存要放在 `env4claude/` 下）
