# OpenClaw triage 命令：专治升级翻车、Doctor 跑不动的诊断修复工具

> 来源：微信公众号（作者未署名）
> 原文链接：https://mp.weixin.qq.com/s/wetVHw9OOR7Thzw2m1h-BA
> 收录时间：2026-09-24
> 类型：OpenClaw CLI · 故障诊断与修复

## 核心功能

`openclaw triage` 是 OpenClaw 2026.9.x 引入的诊断修复工具，三件事串联完成：

1. **读 doctor 体检结果** — 按优先级排列 error/warning，配修复建议
2. **收集脱敏诊断档案** — config 摘要、Gateway 状态快照、日志摘要，全部脱敏
3. **写 AI 修复提示词** — 8KB 以内，大意是"机器病了，你接手，自己修"

---

## 设计亮点：config 坏了也能跑

`triage` 注册时加了 `configGuard: skip`，不依赖 `openclaw.json` 能否正常解析。升级翻车导致配置损坏时，`doctor` 跑不动，`triage` 仍能运行。

---

## 五种使用方式

### 1. 裸跑（自动找 agent）
```bash
openclaw triage
```
按 PATH 顺序探测：Codex → Claude Code → Pi → OpenCode → Muse Code → Grok Build → Cursor → Kimi Code → Qwen Code。找到第一个能启动的就用。

启动 Claude Code 时带 `--safe-mode`，关闭项目 hooks/plugins/skills/MCP server，只留认证和内置工具。

### 2. 指定 agent
```bash
openclaw triage --agent codex
openclaw triage --agent kimi
openclaw triage --agent cursor
```

### 3. 只收集，不启动 agent
```bash
openclaw triage --json        # 输出 JSON（提示词路径、诊断包路径、检测到的 agent 列表）
openclaw triage --non-interactive  # 适合 CI/脚本
```

### 4. 内置 agent 自己修（--run）
```bash
openclaw triage --run
```
用 OpenClaw 配置的模型跑有边界的修复轮次：
- 限制：一轮、共 10 分钟（agent 5 分钟）、最多 40 次工具调用
- 修复前跑 Doctor lint，修复后再跑，对比 error 数量
- 不能和 `--json`/`--non-interactive`/`--agent` 同时用

### 5. 带上更新失败的存档
```bash
openclaw triage --update-result <path>
```
把"从哪个版本升到哪个版本、卡在哪一步"一并塞进提示词。

---

## 脱敏安全设计

| 项目 | 状态 |
|------|------|
| 密钥/Token | ❌ 不含 |
| 原始聊天记录 | ❌ 不含 |
| 原始日志 | ❌ 不含 |
| 绝对路径 | ❌ 打码（相对 `$OPENCLAW_STATE_DIR`） |
| 用户名 | ❌ 不暴露 |
| 提示词文件权限 | 0600（仅自己可读） |

修复阶段更严：
- 外部 agent 走你现有认证和权限
- `--run` 内置修复能动的只有安装目录和暂存候选目录，config 碰不了，Gateway 不许重启

---

## triage vs doctor

| | doctor | triage |
|--|--------|--------|
| 作用 | 体检，只读，报结果 | 体检+开化验单+请主治医生 |
| 输出 | warning/error 列表 | 脱敏诊断包 + AI 修复提示词 |
| 修复 | 你自己看着办 | AI 自动修，修完 Doctor 验证 |
| 依赖 config 正常 | ✅ 是 | ❌ 否（configGuard: skip） |

---

## 适用场景

1. **升级翻车** — OpenClaw 更新到一半挂了，报错看不懂
2. **Gateway 起不来** — 或者起来了但行为怪怪的
3. **懒得自己刨日志** — 一个 error 挂着好几天了

---

## 核实结果（2026-09-24）

### ✅ 核心事实核实

| 原文说法 | 官方来源核实 |
|---------|------------|
| `openclaw triage` 命令存在 | OpenClaw 官方文档确认 ✅ |
| configGuard: skip，config 坏了也能跑 | 官方文档确认 ✅ |
| 五种使用方式（裸跑/指定agent/--json/--run/--update-result）| 官方文档确认 ✅ |
| Claude Code 启动带 --safe-mode | 官方文档确认 ✅ |
| 脱敏：不含密钥/token/原始聊天记录/原始日志 | 官方文档确认 ✅ |
| 提示词文件 0600 权限 | 官方文档确认 ✅ |
| --run 限制：一轮/10分钟/40次工具调用 | 官方文档确认 ✅ |
| Doctor 修复后验证 | 官方文档确认 ✅ |

### 参考来源

- OpenClaw 官方 Triage 文档：https://docs.openclaw.ai/cli/triage
- OpenClaw 官方 Doctor 文档：https://docs.openclaw.ai/gateway/doctor
- OpenClaw 更新故障排查：https://docs.openclaw.ai/install/update-troubleshooting

---

*本文基于 OpenClaw CLI v2026.9.3 版本。*
