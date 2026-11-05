# lifetrail：用 Claude Opus 5.5 做历史人物 3D 人生轨迹视频的 Skill

- **日期**：2026-11-05
- **来源**：微信公众号「AI编程实验室」（作者：鲁工）
- **分类**：02-tools 工具与产品
- **原文**：https://mp.weixin.qq.com/s/Ph3wwxg0Vtu-AdUGl0VZ-Q
- **仓库**：
  - Skill：https://github.com/luwill/lifetrail
  - 源工程：https://github.com/luwill/lives

## 概述

作者 9 月底用 Opus 5.5 做了 11 分钟四渡赤水 3D 地形纪录片，之后延伸出新玩法：让模型按历史人物生平在 3D 地图上绘制人生轨迹视频。已完成毛泽东、诸葛亮、李白、苏轼、朱元璋、徐霞客 6 人，并把整套工程与踩过的 47 个坑打包成 Skill「lifetrail」。

## 技术栈

Three.js 渲染地形 + Playwright 逐帧截图 + edge-tts 配音 + ffmpeg 合成，约 1000 行代码。

作者观点：这套代码 Opus 5.5 从零手搓也能跑通；**真正容易翻车的是史实、纪年、画面细节这类隐蔽坑**，所以才值得封装成 Skill。

## Skill 结构

| 部分 | 作用 |
|------|------|
| `person.json` | 声明式人物配置（用模板重建苏轼全片，抽样三帧像素差均为 0） |
| `SKILL.md` | 8 阶段生成流水线；第 0 阶段先对齐立场、年龄口径与声线，之后自动跑建项 → 史料研究 → 独立核查 → 旁白 → 配乐 → 静帧审片 → 全片渲染 |
| `references/` | 数据规范、核查任务书（**每个里程碑至少两个非维基来源**）、47 条踩坑清单 `pitfalls.md` |
| `template/` | 内置 Three.js 渲染引擎、合成配乐器，156 项测试 |
| `scripts/` | 初始化项目与地形缓存 |

## 使用方法

1. lifetrail 可独立安装：把 GitHub 仓库直接喂给 Claude Opus 5.5
2. 在 Claude Code 里说「给某某做一部人生轨迹视频」即触发
3. 依赖：Node.js、Python 3、ffmpeg、Playwright Chromium

## 备注

- 作者声明：目前仅在 Claude Code + Opus 5.5 组合下跑过，其他 Agent/模型未实测，迁移效果不保证
- 「每个里程碑至少两个非维基来源」的核查任务书设计，是这个 Skill 里比较值得借鉴的做法——把事实核查写进流水线而非靠模型自觉
