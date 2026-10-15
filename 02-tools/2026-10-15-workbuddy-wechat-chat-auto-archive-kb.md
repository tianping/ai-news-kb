# WorkBuddy 实操：微信聊天自动归档，工作对话进知识库（我心飞翔903）

- **日期**：2026-10-15
- **来源**：微信公众号「我心飞翔903」
- **分类**：02 工具与产品
- **原文**：https://mp.weixin.qq.com/s/HjIUas5w8LQnNxTv56i8yA
- **相关**：[wx-cli](2026-10-07-wx-cli-wechat-data-for-agents.md)、[wechat-local-app-guide](2026-10-14-wechat-local-app-guide-chat-history-skill.md)

## 概述

用第三方开源工具 `wechat-cli`（npm `@canghe_ai/wechat-cli`）只读解密本机微信 4.0 数据库，按白名单把指定工作群/联系人的当日对话导出为 Markdown 存入知识库收件箱，再由 AI 清洗出客户决定、行动项、意向，写入客户档案/行动清单，每条结论可回溯到「哪个群、哪一天、谁说的」。每晚定时跑。核心观点：**工具一天能跑通，真正让它长期可用的是外面那层规则**（采什么、不采什么、什么能对外）。

## 原理：六层链路（不是微信机器人）

1. **微信客户端**：聊天记录在本机加密库（macOS 在微信容器目录下 `db_storage`）
2. **wechat-cli**：从运行中的微信进程内存提取密钥，**只读、本地**读取数据库
3. **采集技能**：按自定会话清单导出 Markdown
4. **收件箱留档**：原文按日期归档，一字不改
5. **AI 清洗**：过滤闲聊，提取决定/行动项/意向
6. **知识库**：写入客户档案、行动清单、方法论库，带来源

采集（只搬）与清洗（只整理）分成两步，出问题好定位。不登录微信、不发消息、不碰界面。

## 前置条件

- 微信 **4.0+**（数据目录由 `WeChat Files` 改为 `xwechat_files`，3.x 工具读不到）
- 首次配置时微信须登录运行（从进程内存取密钥）；之后日常采集只读文件，微信开不开都行
- Python 3.10+、npm
- 先想清楚只采哪些工作会话——采集范围 = 风险范围

## 操作步骤（macOS 实测）

1. **安装**：`npm install @canghe_ai/wechat-cli`，`wechat-cli --version` 验证（找不到命令用 `node_modules/.bin/wechat-cli`）
2. **复制微信并重签名**（macOS 默认禁止跨进程读内存）——只改副本，原版不动：
   ```bash
   cp -R /Applications/WeChat.app ~/Applications/
   codesign -d --entitlements - --xml /Applications/WeChat.app > ~/wechat_ent.plist
   # 先从 plist 删掉 application-identifier、keychain-access-groups 等带团队前缀的条目
   codesign --force --deep --sign - --entitlements ~/wechat_ent.plist ~/Applications/WeChat.app
   open ~/Applications/WeChat.app
   ```
   坑：保留团队前缀条目 → 启动即退（错误 163）；删了应用分组权限 → 内嵌浏览器组件可能崩，需同时去沙箱并用参数指定原数据目录启动
3. **初始化**：副本扫码登录后 `wechat-cli init --force`（需管理员权限，建议用系统弹窗授权而非手敲 sudo，以免配置写错位置）；密钥配置存 `~/.wechat-cli/`
4. **验证**：`wechat-cli sessions --limit 8`；查不到多半是名称不对，`wechat-cli contacts --query "关键词"` 查准确显示名
5. **采集清单**：JSON `{"sessions": ["客户A项目群", "内部协作群", ...]}`，写微信显示名；作者只采 6 个，公众号/商会群/业主群一律不采
6. **试跑**：`python3 daily_wechat_ingest.py`；回补 `--start-time 2026-09-20`。按日期建文件夹、每会话一个 md；写《采集登记》页记成功/失败；同名文件跳过，永不覆盖
7. **定时**：launchd/crontab 每天 21:15 采集、21:30 清洗

**Windows**：无需重签名——退出旧版 → 用 4.0 绿色版 `Weixin.exe` 扫码登录 → 管理员命令行 `wechat-cli init` → 后续相同（进程名应为 Weixin 而非 WeChat）

## 关键设计

- 原文层与整理层分离：原文不改留档，结论必须带来源链接，AI 整理错了可回原文纠偏
- 失败只登记不自动重试，避免带病运行

## 六大风险与对策

| 风险 | 级别 | 对策 |
|---|---|---|
| 隐私与授权（对方未同意被机器处理） | 高 | 只采工作会话；清单由负责人审定；告知团队；新会话先过人工 |
| 对外传播 | 高 | 采集授权≠传播授权；公开材料需单独授权+脱敏；联系方式、wxid、内部价格矛盾不进公开材料，案例只写「某行业客户」 |
| 系统安全（重签名/关沙箱放宽保护） | 中 | 只改副本；不在核心资产主力机上做；副本可删即还原 |
| 稳定性（微信自动更新→签名还原、密钥更换） | 中 | 失败只登记；登记页写明需重新提取密钥；修好后回补参数补断档 |
| 第三方工具 | 中 | 只用只读命令（sessions/history/export）；可抓包验证不出网；升级先小范围试 |
| 数据落盘（明文 Markdown） | 中 | 收件箱加访问控制；敏感会话不采或定期归档；外借/送修前清理 |

原则：**先定范围，再定规则，最后才动手。** 范围越窄风险越小，噪声也越少。

## 效果案例

咨询公司创始人，每天几百条微信：每晚自动采 6 个会话，早上 <10 分钟看整理结果；新同事读客户档案即可接手；每月约省 15 小时翻找回忆时间。

**适合**：咨询、销售、项目交付、供应链等以微信为主沟通渠道且需沉淀客户档案的人。**不适合**：私人聊天为主、想要自动回复/群发的、不愿约束采集范围的。

## 备注

- 与 wx-cli（pandorafuture，Rust）同属「解密本地微信库给 Agent 用」路线；本篇偏工作流与风险治理，`daily_wechat_ingest.py` 为作者自写脚本，文中未给源码。
- 文中「WorkBuddy」为标题系列名，正文未展开介绍。
