# DoOps：用「公网 Gateway + 内网 Agent」替代 SSH，运维操作可审计、可交给 AI（智涌君）

- **日期**：2026-11-08（收藏；原文 2026-11-07）
- **来源**：微信公众号「智涌君」（作者：Syman）
- **原标题**：再见，SSH。
- **分类**：02-tools（运维 / Agent 工具）
- **URL**：https://mp.weixin.qq.com/s/KZPcJhiFTv9hakWGVnjaUg
- **项目**：https://github.com/symansun/doops （MIT；核心贡献者孙勇、高家祺、宋晨宇）

## 概述

DoOps 想当 SSH 的替代入口：日常只连公网 Gateway，由 Gateway 路由到内网机器上主动注册的 Agent。目标机器不需要公网 IP、不开端口、不用 VPN，只要能出网。命令执行、文件同步、构建部署走同一条带认证、授权和审计的链路，也给 AI Agent 一个直接操作机器的入口。

文章列的 SSH 痛点：VPN → 跳板机 → 再跳一次的登录仪式；几十套密钥密码散在 Excel 和聊天记录里；没有公网 IP 就得养跳板机/堡垒机；操作无痕，事故复盘靠回忆；AI 拿不到机器，人只能来回复制粘贴。

## 架构：三个角色，连接方向反过来

| 组件 | 作用 |
|------|------|
| `doops` CLI / Skill | 本地唯一操作入口，也是 AI Agent 的调用入口 |
| `doops-gateway` | 公网控制面：认证、授权、审计、排队、路由 |
| `doops-agent` | 跑在目标机器上，主动向外建长连接注册到 Gateway，等任务 |

两种执行模式：

- **快路径**：`exec / read / write / push / pull / info / check / clean`，确定性操作，秒级返回
- **慢路径**：`ask / chat`，把自然语言任务交给远端 doagent 智能体自主分析执行

Agent 容器默认只跑 `doops-agent`、`do-agent`、`buildkitd` 三个进程，不开 SSH、不开 WebIDE。另有 Web Console，与 CLI 走同一条审计链路。

## 上手四步

```bash
# 1. 安装 CLI（~/.local/bin/doops）和 Skill
git clone https://github.com/symansun/doops.git && cd doops
bash scripts/install.sh --project /absolute/path/to/your/project

# 2. 配置 target（token 找管理员要，或 doops login 换取）
doops add --name prod-master --gateway https://gateway.example.com \
  --cluster prod --instance master-1 --aliases prod,master \
  --token '<USER_TOKEN>' --use 'prod cluster via gateway'

# 3. 验证链路
doops list
doops targets --target prod-master        # cluster/instance 在线才算通
doops -session smoke exec --target prod-master --cmd 'hostname'

# 4. 交给 AI
doops -session smoke ask --target prod-master --msg '检查磁盘和内存压力，给出结论'
```

装 Agent：K8s 用 DaemonSet，单机用 Docker/nerdctl 容器，首次自举才用一次 SSH。启动只要 `-gateway-url`、`-cluster`、`-instance` 三个参数，**注册不需要 token**。

## 常用场景

命令统一为 `doops -session <会话> <动作> --target <目标>`：

- **巡检**：`exec --cmd 'uptime && df -h'`，无交互登录，输出可直接给脚本和 AI 用
- **AI 排障**：`ask --msg '最近一小时 502 增多，查原因并给处置建议'`
- **代码同步**：`push --src .` / `pull --dest ./out`，走 Git HTTP 反向隧道，尊重 `.gitignore`/`.doopsignore`；每个会话独立工作区 `/root/ws/<session>`
- **写配置**：`write --path ... --file ./pod.yaml`
- **远端构建**：在 Agent 容器里用 BuildKit `buildctl` 构建并推镜像，宿主机不留构建垃圾
- **批量升级 Agent**：`upgrade --mode k8s ... --dry-run` 先演练；自动 `kubectl set image` + `rollout status`，裸二进制 Agent 会被拒绝

## 二次开发

**写 Skill 沉淀 SOP**：`.agent/skills/<name>/SKILL.md`，frontmatter 含 name、description、`triggers.keywords`、phase、priority，正文写标准工作流和异常红线（如「远端超时或构建依赖错误时立即停止并请求人工审核」）。说到关键词即加载执行。纪律：一个子业务一个目录、远端操作收在 `/root/ws/$SESSION/`、Skill 里禁止写 SCP/SSH 手工步骤、通用能力复用官方 `deploy/SKILL.md`。

**API 对接**：`POST /v1/auth/login` 换 token，之后带 `Authorization: Bearer`；长期自动化由管理员 `POST /v1/admin/tokens` 签发服务 token（明文只返回一次，库里存哈希）。

| 端点 | 用途 |
|------|------|
| `GET /health` | 健康检查 |
| `WS /v1/rpc?cluster=&instance=` | CLI 到 Gateway 的 JSON-RPC（exec/push/ask…） |
| `WS /v1/agent/connect` | Agent 注册长连接 |
| `/v1/git/<cluster>/<instance>/<session>.git` | 代码同步反向隧道 |
| `POST /v1/admin/tokens` | 签发 user token |

401 = token 无效或过期；403 = token 有效但缺该 cluster/instance/action 授权或 admin 权限。详见 `docs/DOOPS_API_GUIDE.md`。

## 安全设计（文章所述）

- 统一用 user token 认证；按 cluster / instance / action 收窄授权，`agent:upgrade`、`admin` 单独授予
- 审计可按 user_id、token_id、cluster、instance、action、session、status 追溯，AI 和人的操作一样可查
- 远端操作收在会话沙盒，高危操作默认拒绝

## FAQ 要点

- SSH 只剩两个用途：首次装 Agent、Agent 挂了时自恢复
- `doops ask` 报错先查 Agent 侧 `doagent-config/settings.json` 的 API Key 和 `API_BASE_URL`；**仓库不发布 `do-agent` 二进制，基础镜像要自己提供**
- 同一 cluster/instance 任一时刻只应一个 Agent 在线，重复注册会替换旧连接
- CLI 预构建覆盖 darwin/linux 的 amd64、arm64

## 我的备注

- 文章是项目方的推介，没有给出 star 数、用户规模或安全审计信息
- Agent 注册不需要 token，加上「重复注册会替换旧连接」——若 Gateway 没有其他校验，知道 cluster/instance 名的人可能冒充 Agent 接走任务；用之前应查清仓库里 Agent 身份是怎么验证的
- 慢路径 `ask` 的远端智能体（do-agent）没开源，需要自备
- 本质是反向隧道 + 统一控制面，同类思路有 Teleport、Tailscale SSH、Cloudflare Tunnel 等；学校 HPC（如 McCleary）一般不允许在计算节点上常驻这类出网 Agent
- 正文由 fetch 脚本抓取，图片未收录
