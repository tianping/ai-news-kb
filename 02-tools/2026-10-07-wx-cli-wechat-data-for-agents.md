# wx-cli：把微信聊天记录变成 AI 能读、能搜、能订阅的数据源

- **日期**：2026-10-07
- **来源**：微信公众号「硅光智读」
- **分类**：02 工具与产品
- **URL**：https://mp.weixin.qq.com/s/44v7TlCMi_MkFHw_gv8HDA
- **项目**：https://github.com/pandorafuture/wx-cli（MIT · Rust · v0.7.4）

## 概述

微信 for Mac 把聊天记录存在本地 SQLite 库里，并用 SQLCipher 加密，官方几乎不提供导出和检索。wx-cli 直接读取本机加密库，把消息、联系人、群聊和媒体交给用户与 AI Agent，提供 CLI、REST API 和 SSE 实时订阅三种出口。数据默认只留在本机。它的定位不是又一个导出器，而是架在微信之上的 Agent 能力层：**只读、不发消息、不模拟登录**。

## 能力

- **查询**：按联系人、群聊、时间范围、消息类型查历史消息
- **全局搜索**：复用微信自带的 `message_fts.db` 全文索引，通常亚秒级返回
- **跨会话时间线**：一次按时间范围拉取所有会话的新消息，适合记忆补全、归档、日报
- **实时订阅**：`watch` 轮询，或通过 SSE 推给 Agent 和其他程序
- **媒体解密**：图片、语音（转 MP3）、视频号视频
- **Agent Skill**：`npx skills add pandorafuture/wx-cli`，Claude Code、Codex、Cursor 装好后就会自己查微信
- **隐私过滤**：隐藏指定联系人、群、标签或群成员

## 上手（macOS Apple Silicon，微信 ≥ 4.1.7）

1. **环境检查**：`wx-cli doctor`。提取密钥需要**关闭 SIP**（恢复模式下 `csrutil disable`），因为 SIP 开着时内核拒绝 `task_for_pid`，root 也不行。LLDB 方式还需要 `sudo DevToolsSecurity -enable`、把用户加入 `_developer` 组、`xcode-select --install`。
2. **安装**：从 GitHub Releases 下载 `macos-arm64` 预编译包，放到 `~/.local/bin/`；也可以 `cargo build --release` 自己编译。
3. **提取密钥**：`wx-cli key extract --timeout 120`。用 LLDB hook PBKDF2 调用，拿到覆盖所有库的 32 字节密钥，过程中会重启微信。已有密钥的话可以跳过 SIP，用 `wx-cli key set <账号> <64位hex>` 手动录入。
4. **查询**（多数命令可以直接读加密库，不必先 decrypt）：
   ```bash
   wx-cli sessions --limit 10
   wx-cli contacts --search 张三
   wx-cli query 张三 --limit 20
   wx-cli search 周末 --limit 20
   wx-cli export 张三 -o /tmp/exp --all --format json
   wx-cli watch --poll --poll-ms 3000
   ```
5. **本地服务**：`wx-cli server run`，默认监听 `127.0.0.1:9100`。远程访问用 `--host 0.0.0.0 --token xxx`，非本机地址时 token 必填。端点有 `/api/v1/{sessions,contacts,messages,timeline,search,media}` 和 SSE 事件流 `/api/v1/events`。所有命令加 `--format json` 都输出结构化数据。

## 架构

密钥提取（wx-keychain：LLDB hook、内存扫描或手动录入）→ 解密（wx-decrypt：SQLCipher KDF 约 25.6 万轮 PBKDF2，新版缓存每个库的派生密钥）→ 查询（wx-db，搜索复用 message_fts.db）→ 监听（wx-monitor：文件变化或轮询）→ 交付（CLI 文本/JSON、REST、SSE）

## 安全与边界

- **主要代价是关 SIP**，等于主动降低一层系统防护。已经有密钥的话可以免关。
- HTTP API **只读**：没有发消息、回复、webhook 等写接口。
- **隐私过滤有漏洞**：`search` 目前**不会**套用隐藏规则，隐藏只对 query、会话、联系人、导出、watch 生效。
- 适合给 Agent 建长期微信记忆、做个人或团队微信搜索、知识库、CRM、自动日报、关键词提醒；想自动发消息、群发的人用不上。

## 备注

- 和本库的 [微信流 WeChatBridge](2026-10-06-wechat-bridge-wechatbridge-ai-chat-forward.md) 是两条思路：WeChatBridge 走微信官方的「转发到其他应用」，不碰数据库，用户手动选聊天；wx-cli 直接解密整个本地库，能力更全，但要关 SIP、拿数据库密钥，隐私风险也更高。
- 目前只支持 macOS arm64。
