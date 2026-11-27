# 未发布的 Claude Opus 5.6 被灰度测出：《最后一张无聊的 PPT》与一批纯代码动画（APPSO）

- **日期**：2026-11-27（收藏）；原文发布 2026-11-27
- **来源**：微信公众号「APPSO」，原标题「Claude Opus 5.6 做出了史上最牛的 PPT，直接杀死比赛？」
- **URL**：https://mp.weixin.qq.com/s/37oyiomvkSeGlFAZwBY-6g
- **分类**：01-models（模型传闻 / 能力观察）
- **作品链接**：https://claude.ai/artifact/Cy2d8Vn63CnzCtonA1LSah （《The Last Boring Slide》）
- **相关**：[Video as Code：用代码生成 MG 动画](../02-tools/2026-10-20-video-as-code-claude-motion-fotor-agent.md)、[SenseNova Code2Video](../02-tools/2026-11-25-sensenova-code2video-open-source.md)、[OpenMontage](../02-tools/2026-10-28-openmontage-agentic-video-production.md)

> ⚠️ **Opus 5.6 尚未正式发布**。「用户在 Opus 5.5 里被分到隐蔽测试的 5.6」的说法来自 X 上的网友和爆料博主，Anthropic 没有官方确认；文中作品是网友自述用 5.6 做的，无法独立验证模型身份。按传闻和社区案例记录。

## 事件

- X 用户 @fluxxrider 发布一个叫《The Last Boring Slide》（最后一张无聊的 PPT）的作品，24 小时内千万次浏览
- 网页形式的演示：有电影感运镜、三维场景、转场，仍可交互，所有视觉效果都由代码实现；作者没有指定风格、没给素材
- 提示词大意：「做一个关于 AI 的未来、基本上就是智能爆炸的 PPT……我希望它是你见过最精心制作、最疯狂惊艳的 PPT，图形和转场随你来。我相信你有品味也有能力。祝你好运，Claude。」
- Claude 用 **42 分钟**交付；作者称用的是在 Opus 5.5 里隐蔽测试的 **Opus 5.6，推理档位 xhigh**，消耗他每周额度的 2–3%，按 Opus 5.5 API 价格折算约 **90 美元**
- 设计巧思：被选中的对象从图表、文字逐渐变成地球、太阳乃至整张画面，用办公软件的视觉符号串起风格各异的场景，像「幻灯片在自己改写自己」

## 其他网友案例（均自称 Opus 5.6，纯代码、无外部素材）

- **鹈鹕骑车系列**（经典 SVG 梗图基准的延伸）：
  - SVG 版「鹈鹕骑车撞墙」碰撞测试
  - 60–75 秒「一本正经的热血体育预告片」，笑点是它把自己看得多认真
  - 年轻人骑巨型鹈鹕穿过集市、码头、夜空的微短片：从平面木刻版画展开成 3D 世界再收回版画；单个 HTML 文件，琴音对应灯笼、笛声控制茶杯蒸汽、低音带动布料树木尾羽，可在浏览器里拖动视角、触发声音
  - 虚构模型「Claude Pelican 1」的宣传片，参照 Claude 官方发布视频风格
- **@ezAIT00**：让模型花两小时做动画，**没用 Blender 也没用 Three.js，从零写了一个 WebGPU 渲染器**，每个像素和声音都由代码生成
- 同一段 Three.js 动画分别出彩色和黑白两个版本——体现代码动画「修改精确」的优点
- **@chetaslua**：上传几张参考图 + 一段提示词，得到 118 秒《龙珠》风格短片：1080P、24 fps、41 个镜头，画面、角色动作、配乐和演唱全由代码完成，144 BPM（涉及现成 IP，版权另说）
- **@fluxxrider** 续作：用 Claude 标志和 Clawd 形象做一段关于 RSI（递归自我改进）的动画，提示词同样很短

## 早期测试者的对比说法

- Opus 5.5 已能胜任大多数动效任务；5.6 动画更流畅，提示词更简单就能讲出连贯故事，能在不用外部素材和工具的情况下完成更复杂的 Three.js 项目
- 也有人说用 Opus 5.5 已经能做出类似短片
- 吐槽：大模型用代码生成的音效「听起来总像闹鬼」

## 用大模型做视频的四种路线（@aura_uniiiii 总结）

| 路线 | 适合 |
|---|---|
| Python | 更自由的视觉风格、手绘质感 |
| Motion Canvas | 批量模板化视频 |
| Three.js | 交互式 3D 场景 |
| Blender | 更复杂的 3D 建模 |

## 顺带提到：Karpathy 的《文明》风格美国数据地图

- Karpathy 分享的新网站：把美国真实统计数据包装成《文明》（Civilization）游戏界面的交互式地图
- 全程用 Claude Code 桌面端完成：**Opus 5.5，推理档位 medium，开启 ultracode**；从下载数据、写清洗脚本到搭前端，约 **7 小时、约 4.5 万行代码**

## 背景

- 原文称距 Fable 5.2 发布约两周；预计 Opus 5.6 会是 Anthropic 上市后发布的第一款模型（原文推测）
- Anthropic 至今没有专门的图像或视频生成模型，这些多媒体作品都是靠写代码（SVG / Three.js / WebGPU / Web Audio）做出来的

## 点评

- 这篇的核心信号不是「PPT 多好看」，而是 **「代码即视频」路线在继续变强**：和本库 [Video as Code](../02-tools/2026-10-20-video-as-code-claude-motion-fotor-agent.md)、[SenseNova Code2Video](../02-tools/2026-11-25-sensenova-code2video-open-source.md) 是同一个趋势——可复现、可精确修改，弱点在写实画面和音效
- 模型身份全靠用户自述，「隐蔽测试」也可能是 A/B 灰度或误认；等 Anthropic 正式发布再核对
- 90 美元一个作品、42 分钟到 2 小时的耗时，说明这类「一句提示词出大作品」目前仍是高成本展示，不是日常生产方式
