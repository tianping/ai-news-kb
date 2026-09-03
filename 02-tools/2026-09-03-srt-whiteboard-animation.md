# srt-whiteboard-animation：SRT 字幕转白板手绘动画 Skill

- **仓库**: [geeklee/srt-whiteboard-animation](https://github.com/geeklee/srt-whiteboard-animation)
- **许可**: MIT
- **作者**: 江哥是老登啊（抖音/B站/公众号同名）——"爱养鱼的老登 / AI Builder / 用 AI 团队打造一人公司"
- **日期**: 2026-09-03

## 是什么

把 **SRT 字幕**自动转成**按叙事顺序绘制的白板手绘视频**：每句字幕对应一个场景元素依次出场，笔尖在区域内**连续落墨（ink → color）**，最终导出 MP4。

**视觉风格**：暖米黄纸张底（建议 `#F5EBD7`）+ 深灰素描线条，红/橙/蓝仅作少量概念点缀；极简手绘、干净背景、充足留白；不用场景文字、摄影感、3D、复杂纹理。

**适用**：知识讲解、故事口播、课程字幕、短视频文案的手绘动画化。

## 核心机制

1. **字幕驱动**：解析 SRT，按建议 25–35 秒拆场景；先出分镜与配图策略（每幕只表达一个核心意思）
2. **语义化排序**：按**字幕事件而非画面坐标**为元素排序——"场景铺垫 → 关键人物/物体 → 动作或变化 → 反应/结果"
3. **annotation.json**：管理区域、时序、字幕关联和重叠保护区（`protectedRegions` 防止后画的内容提前露出）
4. **流式笔迹**：每个区域先 ink 铺线稿再 color 添彩；`direction`/`handPath` 只用于预览台代理，成片笔迹由流式绘制器自动生成
5. **逐步确认**：分镜→线稿→标注→检查图→预览台调整→逐幕渲染→合并，每步都等确认，避免渲染浪费

## 工作流命令

```bash
# 环境（独立 venv）
python scripts/prepare_env.py --check
python scripts/prepare_env.py   # 输出 ENV_PY=<路径>，后续用它

# 解析字幕 → 分镜建议
python scripts/parse_srt.py <字幕.srt> --target-sec 30 --min-sec 25 --max-sec 35

# 标注检查图
python scripts/render_annotation_preview.py <图片> <标注.json> <预览图>

# 本地预览台：浏览器打开 assets/preview.html，"打开文件夹"载入场景目录

# 渲染单幕
<ENV_PY> scripts/render_stream_whiteboard.py <图片> <标注> <输出.mp4> assets/drawing-hand.png \
  --ink-path grid --color-fill contour-wipe
# 线稿清晰时可换 --ink-path skeleton

# 合并多幕
<ENV_PY> scripts/merge_scenes.py --inputs 幕1.mp4 幕2.mp4 --output final.mp4
```

## 目录约定

```
assets/whiteboard/<项目名>/
├── scene-01-<名称>.png
├── scene-01-<名称>.annotation.json   # 必须与图片同名
├── scene-01-<名称>-whiteboard.mp4
└── scene-01-<名称>-preview.mp4
```

annotation 关键字段：`sequence` / `subtitle` / `narrativeRole` 关联字幕；`region` 用整数像素坐标；`reveal`（direction、startMs、durationMs、maskPaddingPx、protectedRegions）；`handPath`（start/end/easing）。

## 验收清单（README 原文提炼）

- 首帧是干净的纸张底色，无提前露出的线条
- canvas 与原图尺寸一致，区域都在画布内
- sequence / startMs 与字幕叙事顺序一致
- 中段帧里未开始区域和保护区不提前出现
- 每幕结束至少停留 0.5 秒完整画面

## 关联

- 本工作区技能 `story-to-handdrawn-video`（彩铅日记 20 风格库）做的是同类"手绘故事视频"——srt-whiteboard 走**字幕驱动 + 遮罩流式笔迹**路线，可互补参考其 annotation 设计和预览台思路
