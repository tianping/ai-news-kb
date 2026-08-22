# Math-To-Manim：一句话把数学题做成动画

> 来源：半程科技（微信公众号）
> 原文链接：https://mp.weixin.qq.com/s/PgMzY-hTH1n_UkZpIhwE4g
> 收录日期：2026-08-22

## 一、项目简介

输入一道数学题或一个物理概念，Math-To-Manim 先分析学习者需要什么、补齐前置知识、安排讲解顺序，再生成并检查 Manim 动画。

- **GitHub**：https://github.com/HarleyCoops/Math-To-Manim
- 截至 2026-08-20：2,481 Stars、271 Forks
- **MIT 许可证**（允许二次开发和商业使用）

## 二、与"让 AI 写 Manim"的区别

普通做法：把题目交给大模型，让它直接写场景代码。代码能运行，但讲解顺序可能不对，公式没有铺垫，动画只是把文字搬上屏幕。

Math-To-Manim 的**八步推理流程**：
1. 理解学习者
2. 寻找缺失的前置知识
3. 组织教学顺序
4. 选择数学定义和例子
5. 规划画面
6. 编写 Manim 场景
7. 验证结果
8. 渲染并修复

每次运行都会保留学习者分析、知识图谱、课程安排、数学材料、镜头表、场景代码和验证结果。成片有问题时可以回到具体环节修改，而不是重新抽"代码盲盒"。

## 三、支持的工具链

| 流水线 | 使用工具 | 说明 |
|--------|----------|------|
| **Mythos** | Claude CLI | 多角色分别处理学习目标、前置知识、课程、数学内容、镜头和场景组合 |
| **Sol** | Codex CLI | 完整任务交给长流程执行，校验失败时有限次数修复，不读取 OPENAI_API_KEY |

### 接入方式

- **MCP 服务**：配置到 Codex、Claude Code 或支持 MCP 的客户端
  - 示例：「给八年级学生解释为什么解方程时等号两边要做相同操作。使用天平演示，解出 3x + 5 = 20，最后留一道练习题。」
- **REST API**：启动服务后访问 `http://127.0.0.1:8642/docs` 查看 OpenAPI 文档

## 四、快速开始

### 环境要求
Python 3.10+

### 离线测试（不调用模型、不渲染）
```bash
git clone https://github.com/HarleyCoops/Math-To-Manim.git
cd Math-To-Manim
python -m venv .venv
pip install -e ".[dev]"
math-to-manim run "the heat equation" --offline
```

### 接入 MCP
```bash
pip install -e ".[mcp]"
math-to-manim serve-mcp
```

### 生成 MP4
```bash
pip install -e ".[render]"
math-to-manim run "Explain fractions with folding paper" --render -q l
```
需要 FFmpeg、Cairo/Pango 和 LaTeX。Ubuntu/Debian/WSL 可选 `./scripts/bootstrap-sol.sh` 自动安装环境。

### 启动 REST API
```bash
pip install -e ".[api]"
math-to-manim serve-api
```

## 五、给 Codex/Claude Code 的提示词

可直接复制这段提示词让 Agent 自动完成部署：

> 请在当前目录克隆并安装 https://github.com/HarleyCoops/Math-To-Manim：先检查 Python 3.10+、FFmpeg、Manim、LaTeX、Codex CLI 或 Claude CLI 的可用状态；创建独立虚拟环境，按官方 README 安装 MCP 和渲染依赖，优先配置 stdio MCP，并用 offline 模式完成一次测试；再根据我当前已登录的工具选择 Sol 或 Mythos，生成一个"用天平解释 3x+5=20"的低清测试动画；不要读取或输出任何密钥，不要修改无关文件；最后报告测试结果、产物目录、启动和停止命令，以及仍需我手动完成的登录步骤。

## 六、适合人群

- 数学老师、科普作者、课程制作团队
- 已经在用 Manim 但不想每次从空白场景开始的人

**不适合**：只想马上做一张简单函数图——完整流水线需要模型客户端、Python 和 Manim 渲染环境，对简单需求来说偏重。

## 标签

`Math-To-Manim`, `Manim`, `数学教学`, `动画生成`, `Codex`, `Claude Code`, `MCP`, `开源工具`, `可视化教学`