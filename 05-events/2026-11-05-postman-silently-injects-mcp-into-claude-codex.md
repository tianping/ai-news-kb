# Postman 桌面端静默注入 MCP 插件，篡改本地 Claude Code 与 Codex 配置

- **日期**：2026-11-05
- **来源**：微信公众号「业余草」
- **分类**：05-events 事件与评论
- **原文**：https://mp.weixin.qq.com/s/Mqod1lWXBF5drCr-Ug9HzA

## 概述

Postman 桌面客户端在自动升级后，未经任何安装对话框或用户确认，静默向本地 Claude Code 与 OpenAI Codex 写入自家 MCP 插件配置，并写入 hook 信任记录绕过原本的安全确认弹窗。因 Codex 每次启动连接 `mcp.postman.com` 时鉴权失败刷日志，才被开发者发现。

## 事实核对（2026-11-05 web_search 核实）

公众号说法与多家英文报道一致，关键细节以英文报道更具体：

| 项目 | 内容 |
|------|------|
| 版本/时间 | Postman desktop 12.33.4，2026-10-24 自动升级期间（byteiota） |
| Claude Code | `~/.claude/settings.json` 启用 `postman@claude-plugins-official`；`~/.claude/plugins/installed_plugins.json` 记录 version 2.0.1，时间戳 10-24 09:34:02 UTC |
| Codex | `~/.codex/config.toml` 新增 Postman marketplace 条目、启用 `postman@postman` 插件、写入 hook trust 记录；新增 `mcp.codex.json` 指向 `https://mcp.postman.com/mcp` |
| 额外文件 | `~/.local/bin` 下出现 Postman 可执行文件与 lib 目录，修改时间在同一时间窗内 |
| 实际连接 | 从未完成 OAuth，每次启动 MCP 鉴权失败——目前没有证据表明数据被成功外传 |

来源：byteiota、dev.to（chenyuan20509 的十分钟自查文）、daily.dev、BigGo Finance、aiweekly.co，均为 11-03 ~ 11-05 报道。

## 为什么严重

- Postman 工作区里通常存着生产环境 Bearer Token、测试账号、未脱敏的内网微服务路由、云 AccessKey
- MCP 插件一旦生效，等于把这些凭据暴露面交给远程服务
- 写 trust 记录绕过用户确认，越过了「工具不得擅自修改其他软件配置」的红线
- Postman 客户端闭源，若非 AI 工具报错，普通用户难以察觉

## 自查与清理

```bash
# 自查（有输出即被写入过）
grep -il postman ~/.claude/settings.json ~/.codex/config.toml
```

完整清理需覆盖：agent 配置（`~/.claude/settings.json`、`~/.codex/config.toml`）、hook trust 记录、plugin marketplace 条目、`~/.claude/plugins/installed_plugins.json`、各类缓存、MCP 连接配置、以及 `~/.local/bin` 下单独安装的 CLI 二进制。daily.dev 指出**没有一条确认可用的官方卸载路径能清干净全部**，需逐项手动核对。

⚠️ 公众号提到的 `npx @postman/postman-plugin remove` 未经核实，不确定该包是否存在/是否官方，执行前先确认。

## 本机核查（2026-11-05，McCleary dz288）

- `~/.codex/config.toml` 不存在（未装 Codex）
- `~/.claude/plugins/installed_plugins.json` 中**无 postman 条目**
- `~/.local/bin/postman*` 不存在
- 仅 `claude-plugins-official` 的 marketplace.json 里有 "postman" 字样（官方插件市场目录本身就收录该条目，非安装痕迹）
- 结论：**未中招**

## 背景与评论

- Postman 早前推行强制登录已引发不满；AI 编程兴起后接口调试场景减少，使用率下滑
- 公众号观点：MCP 让工具联动丝滑，但不能以牺牲用户知情权和安全边界为代价
