# chinese-poetry：从唐诗到元曲，37 万首古诗词，一键接入你的代码

## 项目介绍
chinese-poetry 是国内知名开源古典诗词数据集，汇总整理历朝古典文学作品；
chinese-poetry-api 基于这份数据集二次开发，封装成开箱即用的 HTTP API 服务。
整套资源合计收录37 万首古典诗词，覆盖唐诗、宋词、元曲、古文等体裁，包含 13 万余名诗人，横跨 11 个朝代、划分 17 大类题材。
两种使用方案自由选择：直接部署 API 在线调用；或是下载原始 JSON 数据，导入 MySQL、SQLite、向量数据库本地私有化使用。
非常适合开发诗词小程序、诗词问答 AI、诗词卡片生成器、学习工具、AI Agent 知识库等项目。

## 核心优势
### chinese-poetry：最全的中文古诗词数据库
仓库地址：https://github.com/chinese-poetry/chinese-poetry
这个项目在 GitHub 上非常有名，它做的事情就是一件事——整理中国古典诗词的开放数据。
数据规模：
• 📚 37 万首诗词：唐诗、宋诗、宋词、元曲、诗经、楚辞、论语、四书五经、蒙学……全覆盖
• 👤 13000+ 诗人：从李白杜甫到冷门诗人，一网打尽
• 🏛️ 11 个朝代：先秦到清代
• 🏷️ 17 种题材：五言绝句、七言律诗、乐府、词牌……数据以 JSON 格式存储，结构清晰，字段规范。你完全可以直接 clone 下来导入自己的数据库，想怎么用怎么用。

### chinese-poetry-api：基于 Go 的高性能 API 服务
GitHub：https://github.com/palemoky/chinese-poetry-api
在线体验：https://poetry.palemoky.com
chinese-poetry 只给了「数据」，但没有「接口」。
chinese-poetry-api 正好补上了这个缺口——它把 chinese-poetry 的数据做成了可直接调用的 API 服务，基于 Go 语言编写，性能拉满。

## 功能介绍
API 能做什么？基本上你能想到的诗词查询，它都支持。

### 随机一首诗
GET /api/v1/poems/random
每次请求返回一首随机诗词。如果带上参数，还能更精准：
- ?author=李白 — 只看李白的诗
- ?dynasty=唐 — 只看唐代
- ?type=五言绝句 — 只看五绝
- ?char=春 — 飞花令！随机返回包含"春"字的诗词

### 全文搜索
GET /api/v1/poems/search?q=床前明月光&type=content
搜索类型支持四种：
- all：全文搜索
- title：按诗名搜
- content：按内容搜
- author：按作者搜
搜索基于 SQLite 的 FTS5 trigram 索引，性能很棒，秒级返回。

### 作者查询
GET /api/v1/authors?page=1&page_size=20
GET /api/v1/authors/{id}
分页列出所有作者，也能按 ID 查某个诗人的详细信息。

### 统计信息
GET /api/v1/stats
返回诗词总量、作者数量、各朝代分布……做数据大盘或者首页展示很好用。

### 简繁体切换
所有接口都支持 ?lang=zh-Hant 参数，同一份数据同时存储简体和繁体，切换自如。

### GraphQL 接口
POST /graphql
适合前端需要灵活字段组合的场景，只查想要的字段，减少数据传输量。

### 飞花令功能
带 ?char=春 参数请求随机接口，返回包含指定字的诗句——做互动游戏、小程序、教育应用的绝佳素材。

### 简繁一体
同一数据库同时存简体和繁体，?lang= 参数切换，转换性能 ~300ns/op。对港澳台用户友好，做国际化也不用额外折腾。

## 快速上手
### 方式一：部署 API 服务（快速开发首选）
1. 克隆 chinese-poetry-api 项目
   ```bash
   git clone https://github.com/palemoky/chinese-poetry-api
   ```
   项目会自动拉取 chinese-poetry 诗词数据集；
2. 安装依赖，启动服务；
3. 直接 HTTP 请求接口示例：
   ```bash
   # 获取随机一首诗词
   GET /api/poem/random
   # 查询苏轼全部诗词
   GET /api/poet/search?name=苏轼
   # 关键词检索诗句
   GET /api/poem/search?keyword=春风又绿江南岸
   ```
   随机一首诗词返回结果：
   ```json
   {
     "id": 12345,
     "title": "静夜思",
     "author": "李白",
     "dynasty": "唐",
     "content": ["床前明月光，", "疑是地上霜。", "举头望明月，", "低头思故乡。"],
     "type": "五言绝句"
   }
   ```

### 方式二：纯本地数据集方案（完全私有化）
1. 克隆原始数据集仓库
   ```bash
   git clone https://github.com/chinese-poetry/chinese-poetry
   ```
2. 本地得到海量 JSON 诗词文件；
3. 编写导入脚本，批量存入 MySQL/PostgreSQL/MongoDB/向量数据库；
4. 自主开发查询逻辑，不再依赖第三方接口，数据完全掌控。

## 最后
适合做什么？
- 🖥️ Chrome 新标签页插件：每日一首诗，简洁文艺
- 📱 古诗词学习小程序：搜索、收藏、背诵、飞花令
- 🎮 文字互动游戏：飞花令对战、诗词接龙
- 🤖 AI 对话机器人：给 ChatGPT 加诗词知识库
- 📊 数据可视化：朝代诗词分布、诗人社交网络
- 🏫 教育平台：古诗词题库、背诵检查
- 🔧 个人项目练手：前后端全栈练习的好素材

这两个项目都是 MIT / 开源协议，数据来自公开的古籍整理，自己用、做项目都没问题。但做商业产品的话，建议确认一下数据来源的授权细节——chinese-poetry 的 README 里有说明。

---
*来源：微信公众号文章 “chinese-poetry：从唐诗到元曲，37 万首古诗词，一键接入你的代码”*
*收藏时间：2026-08-20*