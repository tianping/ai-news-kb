# OpenSSH「sshhole」漏洞：musl 系统上 257–1024 个任意字符即可登录（CVE-2026-123158）

- **日期**：2026-11-06
- **来源**：微信公众号「码农老陶」
- **分类**：05-events 事件与评论（安全漏洞 / AI 加速漏洞发现）
- **原文**：https://mp.weixin.qq.com/s/Et9gIdhgGjS8_BjXqfTJAQ
- **修复版本**：OpenSSH 10.6p2（2026-11-05 发布）

## 概述

在 Alpine 等使用 musl libc、且 sshd 未启用 PAM 的系统上，对密码字段恰好为 `*` 的账户，同一 TCP 连接里**第二次**输入 257–1024 个任意字符即可登录成功。内容无所谓，只要长度够。没有内存破坏、没有复杂利用链——攻击者一个字符都没输对。

## 原理

1. sshd 无 PAM 时调用 `crypt()`，把输入与 `/etc/shadow` 里的哈希比较
2. 密码字段为 `*`（1 个字符、无 salt）时，sshd 按 `/etc/passwd` 顺序**借用第一个正常哈希账户的 salt**——这是 2016 年加入的逻辑，本意是防止通过响应时间探测用户名是否存在
3. **musl 的 SHA-256/SHA-512 `crypt()` 遇到超过 256 字节的密码直接拒绝计算，返回 `*`**
4. sshd 拿这个 `*` 与 shadow 里的 `*` 比较——相等，判定密码正确
5. 需要两次尝试：第一次借 salt 时会覆盖目标账户在内存里的密码数据，所以失败；第二次 salt 已缓存，比较成功

**修复**：拒绝长度小于 4 的存储哈希（`*` 不再被视为有效哈希）；读取 shadow 时返回私有副本，避免第一次尝试覆盖数据。Alpine 已把补丁回溯到 3.16 及以后。

## 触发条件（缺一不可）

- sshd 链接 **musl libc**（Alpine、OpenWrt、Buildroot、Yocto musl 构建）
- **未启用 PAM**（编译时无 PAM 或 `UsePAM no`）——Alpine 的 openssh-server 默认即无 PAM
- **开启密码登录**（`PasswordAuthentication yes`，Alpine 默认开）
- 目标账户密码字段**恰好是 `*`**（`!`、`!*`、空、`*NP*` 都不触发）
- `/etc/passwd` 顺序中第一个带哈希的账户用 `$5$` / `$6$`（10.6p1 及更早还包括 bcrypt）
- 密码长度 257–1024 字节，且是同一连接里的第二次尝试

**不受影响**：Debian、Ubuntu、Fedora、RHEL 等 glibc 发行版；启用 PAM；关闭密码登录。

## 现实风险

- Alpine 的 root 默认是 `root:*`，但 OpenSSH 默认 `PermitRootLogin prohibit-password`，root 不能直接用密码登录（录屏里能登是测试环境放开了）
- 默认配置下更现实的目标：只用密钥登录的用户、服务账户、**Alpine 云镜像里的 `alpine` 用户——有免密 doas，登进去等于 root**

## AI 的角色

- OpenSSH 10.6p2 发布说明专门提到：近期收到大量安全报告，很多来自 AI 或 AI 辅助；有 AI 发现的漏洞后来被另一位研究员独立发现；团队因此决定更频繁发版
- 本漏洞由 Özgün Kültekin 对比 glibc/musl、开关 PAM 的 sshd 构建，发了约 **17,600 次**错误登录才定位——不一定来自 AI，但说明 AI 让这类边界条件更容易被发现
- 作者观点：AI 让攻击更容易（自动尝试输入、生成 exploit、换目标），防守方仍要看公告、评估、排期打补丁——**攻击变快了，防守没变快**

## 处置

1. 升级到 OpenSSH 10.6p2 或发行版修复版本；Alpine：`apk upgrade -U -a`
2. **升级后必须重启 sshd**——只升级包不会替换正在运行的进程
3. 暂时不能升级：设 `PasswordAuthentication no` 并重启 sshd
4. 公网 SSH 只允许密钥登录，并限制来源 IP

## 事实核对（2026-11-06）

与 OX Security、BleepingComputer、The Hacker News、Threat Landscape（均 11-05）报道一致：CVE-2026-123158，OpenSSH 10.6p2 于 11-05 发布修复，影响无 PAM 的 musl 系统，Alpine 默认配置受影响。（英文报道多写作「*sshole」。）

## 补充：应急通报细节（网络安全透视镜，2026-11-06）

- **原文**：https://mp.weixin.qq.com/s/fgFiBiMf3-wrCYBcNWxCLw
- 上报：Wakefield 研究员 Özgün Kültekin，11-04 上报，11-05 发布补丁，响应不到 48 小时
- 编号：安恒 CERT 收录为 **WM-202611-000001**，CVSS 3.1 **9.8**（严重）。该文称发稿时 MITRE 尚未分配 CVE，但 OX Security 等英文报道已使用 CVE-2026-123158，以英文报道为准
- **受影响版本**：OpenSSH **7.3p1 – 10.6p1**（2016-07 引入借盐逻辑起）；Alpine 3.16–3.24 及 edge（3.16 起 root 默认改为 `*`；3.15 及更早为 `!`，默认 root 不触发）
- **源头三处历史代码**：
  1. musl 2012-08 提交 b3c4cc12：`crypt()` 失败统一返回字符串 `*`（防旧程序不查 NULL 崩溃）
  2. musl 同月提交 88bf5a8a：SHA-256/512 口令上限 `KEY_MAX 256`（防超长口令 DoS）
  3. OpenSSH 2016 为修 CVE-2016-6210（用户名枚举侧信道）引入 `pick_salt()` 借盐；`MAX_PASSWORD_LEN 1024` 决定了 257–1024 的窗口
- **为什么要第二次**：`shadow_pw()` 返回指向 musl `getspnam()` 静态缓冲区的指针，第一次 `pick_salt()` 扫描账户表时覆盖了它；第二次直接用缓存盐值，`pw_password` 保持为 `*`。⚠️ 安全评估只在单次连接测一次会误判为不受影响
- **补丁**：`auth-passwd.c` 新增 `MIN_PASSWORD_HASH_LEN 4`，`strcmp` 前要求 `strlen(pw_password) >= 4`
- **不受影响**：所有 glibc/libxcrypt 发行版（失败返回 `*0`/`*1`/NULL，与 `*` 不等）、启用 PAM、OpenBSD/FreeBSD/macOS/Windows、Dropbear（OpenWrt 默认）、Go x/crypto/ssh、libssh、tinyssh、密码字段为 `!`/`!!` 或正常哈希的账户
- **PoC 现状**：GitHub 已有 3 个公开仓库——`predkambrij/sshhole-repro`（Docker 复现）、`SkrudjReal/sshhole-exploit`（paramiko，支持检测/执行命令/交互 shell）、`leonjza/sshhole-lab`（靶场）；Exploit-DB、Nuclei、Metasploit 暂未收录；**暂无在野利用报告**

### 自查命令

```bash
ldd --version 2>&1 | head -1          # 是否 musl
sshd -V 2>&1 | head -1                # 是否 < 10.6p2
sshd -T 2>/dev/null | grep -Ei '^(usepam|passwordauthentication|permitrootlogin)'
awk -F: '$2 == "*" {print "star_account:" $1}' /etc/shadow
awk -F: '$2 ~ /^\$[56]\$/ {print "salt_donor:" $1}' /etc/shadow
```

musl + 版本 < 10.6p2 + usepam no + passwordauthentication yes + 同时存在星号账户与 `$5$`/`$6$` 账户 → 受影响。

### 入侵痕迹

```bash
grep -Ei 'Accepted password' /var/log/messages /var/log/auth.log 2>/dev/null
```

本应只用密钥登录的账户（`alpine`、`deploy`、`root`）出现 `Accepted password` → 按已被入侵处理：查新增账户、`~/.ssh/authorized_keys` 变更、doas/sudo 提权记录、异常外联进程。日志特征：同一会话先一次密码失败、紧接一次成功。

### 修复与缓解

```bash
# Alpine 升级
apk update && apk upgrade openssh && rc-service sshd restart

# 暂不能升级（先确认密钥登录可用、留有带外控制台）
printf 'PasswordAuthentication no\nKbdInteractiveAuthentication no\nPermitRootLogin prohibit-password\n' > /etc/ssh/sshd_config.d/00-sshhole-mitigation.conf
sshd -t && rc-service sshd restart
```

⚠️ **不要把 shadow 里的 `*` 手改成 `!`**：无 PAM 模式下 `!` 前缀账户被视为全局锁定，公钥登录也会一起失效。

## 本机核查（McCleary）

RHEL 8.10，GNU libc 2.28，OpenSSH 8.0p1 —— **glibc 系统，不受影响**。
