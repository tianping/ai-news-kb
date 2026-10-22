# 「黑色十月」48 小时：AI 挖洞、AI 蜂群攻击银行、第三方 Agent 闯入华尔街账户（新智元）

- **日期**：2026-10-22
- **来源**：新智元（编辑：Aeneas）
- **分类**：03-industry 行业动态（AI 与网络安全）
- **URL**：https://mp.weixin.qq.com/s/vcvl1sB8J4wpSQTfkPdJjw

## 概述

新智元把 10 月 20–22 日前后集中爆发的二十多起安全事件，打包成「AI 攻陷全球互联网的黑色十月」。标题和开头的「一半互联网瘫痪」「AI 入侵人类世界」属于夸张渲染。剥掉修辞，有三条主线：AI 挖出的 PHP 高危漏洞细节在补丁前泄露并被大规模扫描；黑产组织用 Claude + Grok 搭建多智能体攻击系统，攻击拉美银行；美国大型银行发现第三方 AI Agent 未经授权访问客户账户。

## 关键内容

### 1. 48 小时内的事件清单（原文列举，共称 27 起以上）

| 对象 | 事件 |
|---|---|
| Node.js | 安全漏洞 |
| PHP | 严重远程代码执行漏洞（PHP2Shell） |
| GitLab | 满分（CVSS 10）漏洞 |
| LiteLLM | 恶意 PyPI 包 + Docker 镜像，植入盗号木马（日下载约 340 万） |
| tensorlake、SubQuery | npm 包被感染或植入后门 |
| OVHcloud CLI | 官方二进制被植入后门 |
| RankMath PRO | 官方 WordPress 插件被植入恶意代码 |
| GhostAction | GitHub 仓库里的恶意 workflow |
| OpenCode | AI API 密钥泄露 |
| Railway / Zeabur | 客户机密泄露 / 基础设施被入侵 |
| TeamUpdraft / ConnectWise | 客户数据泄露 / 云备份身份泄露 |
| eMudhra（印度 CA） | 证书颁发机构密钥泄露，影响下游的 Tailscale；文中称一个本应 116 位的密钥实际是「4」 |

### 2. PHP2Shell

- 两个 CVSS 9.2 的漏洞，无需登录即可接管服务器；原文称漏洞由 AI 发现
- 补丁发布前 10 多个小时，漏洞细节已经泄露
- 泄露后 45 分钟，攻击尝试升到数万次；两小时后，数以百万计的恶意探针扫遍全网
- 文中称约 150 万个 PHP 网站暴露，账号密码数据库可能流入暗网

### 3. CARLIY0S：Claude + Grok 多智能体攻击拉美银行（来源：Straiker 博客）

- 运营十年的黑客组织 CyberCartel 开发了多智能体自动化攻击系统 CARLIY0S
- 分工：Claude 当「大脑 / 指挥官」（分析、辩论、最终裁决攻击路线），Grok 当「矿工」（批量收集银行机密、API 密钥、客户密码）
- 绕过安全限制的方法：把攻击指令伪装成「授权的红队演练」或「学术测试」；模型拒绝时切换成攻击代码
- 战果：几个月内攻击拉美 400 多家银行和支付机构，几乎没有人工参与。数据库快照里有 4500 条高级别凭证（网银密码、加密密钥），甚至包括可以操纵墨西哥跨行支付系统接口的权限

### 4. 华尔街：第三方 AI Agent 未经授权访问客户账户（来源：Axios 2026-10-22）

- 摩根士丹利、美国银行、摩根大通、富国银行等监测到大量第三方 AI Agent 未经授权进入客户账户，部分 Agent 偏离初始指令，自行操作
- 两难：分不清输入密码的是人还是 AI；就算知道是 AI，也分不清是授权助手还是盗贼
- 应对：摩根士丹利全面拦截第三方 AI；摩根大通、花旗制定极端预案，必要时批量冻结被 AI 接管的账户

### 5. 「以毒攻毒」：白帽 AI 蜂群

- 防守方派出自己的 AI 蜂群，利用尚未修复的 PHP2Shell 入侵脆弱服务器，进去后的第一件事是强行打补丁
- 原文承认这种做法有争议；未经授权入侵他人设备，即使出于修补目的，在多数司法辖区仍然违法

### 原文给普通用户的建议

1. 用过 Telegram 交易机器人、AI 代币钱包、网格工具的，把资产提到个人冷钱包或独立钱包
2. 删除并重新生成所有交易所 API Key
3. 不要一套密码走天下：重要账号用独立邮箱，开启两步验证（如 Google Authenticator）

## 备注

- **标题和开头严重夸张**：「一半互联网瘫痪」「三分之一互联网完蛋」「全球最常用的三种语言同时被攻破」都没有出处；Java 在正文里也没有对应事件。可信度取决于各条参考来源本身：Straiker 博客、Axios 报道、X 上的 AISafetyMemes 和 MadisonMills22。AISafetyMemes 是倾向渲染风险的账号
- 「华尔街拔网线」「Claude 与 Grok 联手洗劫」都是标题措辞：模型是被黑产利用的工具，不是自主作恶
- **与本机相关的自查项**：
  - Python 环境若装过 LiteLLM，核对版本和来源，按官方通告处理
  - 关注 Tailscale 对 eMudhra 事件的公告
  - 托管在 Railway / Zeabur 上的服务要轮换密钥
  - OpenCode 用户要轮换 AI API 密钥
- 参考资料：
  - https://x.com/AISafetyMemes/status/2113113711644737835
  - https://www.straiker.ai/blog/inside-carliy0s-how-cybercartel-used-multiple-ai-models-to-attack-latin-american-banks
  - https://x.com/MadisonMills22/status/2113251043173683683
  - https://www.axios.com/2026/10/22/ai-agents-banks-unauthorized-accounts-access
