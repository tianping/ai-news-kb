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

## 本机核查（McCleary）

RHEL 8.10，GNU libc 2.28，OpenSSH 8.0p1 —— **glibc 系统，不受影响**。
