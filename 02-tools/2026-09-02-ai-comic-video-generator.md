# AI-Comic-Video-Generator：全栈开源 AI 漫剧生成平台

- **仓库**: [cleversnail/AI-Comic-Video-Generator](https://github.com/cleversnail/AI-Comic-Video-Generator)
- **来源**: [微信公众号文章](https://mp.weixin.qq.com/s/29S6_7WAcOvQ6n2p-02iBA)
- **日期**: 2026-09-02
- **定位**: 自托管 AI 漫剧流水线——数据、模型选择、API Key 全在自己手里

## 核心思路

输入角色设定 + 故事文本 → 系统自动完成**分镜拆解 → 画面生成 → 配音合成 → 视频输出**，全流程自动化。

**最大卖点：不绑定任何 AI 服务商**。每个环节用什么模型自己选、API Key 自己填：文案可用 DeepSeek，画图可用 FLUX，视频可用可灵，配音可用 MiniMax TTS——想省钱或堆质量都行，平台只负责调用不赚差价。

## 七大核心功能

1. **3 步出草稿**：创建项目输入角色 → 粘贴故事点"生成分镜" → 预览/单镜重生成 → 一键合成。熟练 3 分钟出第一条草稿
2. **AI 自动分镜**：LLM 把故事拆成 4–8 个专业分镜（场景描述、镜头角度、人物动作、台词），可手动调整
3. **多模型自由组合**：LLM / 图像 / 视频 / TTS 各环节独立选模型
4. **角色一致性管理**：每个角色建详细提示词卡（外貌/服装/发色瞳色），生成分镜时自动带上——解决"上个镜头长发下个镜头短发"的经典翻车
5. **实时预览 + 单镜重生成**：不满意只重做那一镜，不用整批重跑，省时间省 token
6. **异步任务队列**：BullMQ + WebSocket 实时推送进度，支持多任务并发
7. **FFmpeg 合成导出**：多分辨率多格式，导出即用

## 技术架构

| 层 | 技术栈 |
|---|---|
| 前端 | Next.js 14 + Tailwind CSS + Zustand + Framer Motion |
| 后端 | NestJS + Prisma + MySQL |
| 队列/缓存 | Redis + BullMQ（Redis 5.0+ 为硬性要求） |
| 存储 | MinIO 或 S3 兼容 |
| 安全 | JWT 认证；API Key 用 **AES-256-GCM** 加密存储；图形验证码 + 接口限频 |

**亮点设计——AI 适配器层**：LLM/图像/视频/TTS 每类模型定义统一接口，接新模型只需写一个适配器实现类，不动业务代码，扩展成本低。

## 部署速览

环境：Node.js 18+、pnpm 8+、MySQL 8.0+、Redis 5.0+（BullMQ 要求）、MinIO/S3（可选）

```bash
# 后端
cd apps/ai-video-api && pnpm install
# 前端
cd ../ai-video-web && pnpm install
# 配置 .env（数据库/Redis/JWT 密钥），迁移 + 种子数据
cd apps/ai-video-api
npx prisma migrate dev && npx prisma db seed
# 启动：后端 :3001 / 前端 :3000，Swagger 文档 :3001/api/docs
```

## 使用流程

注册登录 → 模型中心填 API Key（加密存储）→ 创建项目 →「角色」Tab 建角色卡 →「故事」Tab 粘贴文本生成分镜（十几秒）→ 不满意单镜重生成 →「生成」页创建任务 → 导出下载。

## 适合谁

- **AI 漫剧创作者**：不想被 SaaS 平台限制，数据和模型自选
- **小团队/工作室**：内部生产工具，多人协作统一管理模型配置
- **技术学习者**：全栈 AI 视频生成的架构参考（适配器层设计值得看）
- **二次开发创业者**：代码开源可直接改，省从零搭建时间

## 评价

- Star 数不高但完成度不错，核心链路已跑通
- 不适合纯小白，需要一定技术基础
- 意义在于：漫剧赛道大多是闭源 SaaS，这个项目提供了"自己部署、自己管理"的另一个选项
