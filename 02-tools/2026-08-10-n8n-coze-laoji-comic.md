# n8n+Coze实战：自动复刻老纪先生漫画，直通公众号草稿箱

> 来源：[n8n+Coze实战：扔个标题，3分钟复刻老纪先生漫画，直通草稿箱！](https://mp.weixin.qq.com/s/Or1j-n7szFGZHKmiLwGWYw) · 2026-08-10 · 后端小肥肠

## 一句话简介

用 n8n + Coze 搭建自动化工作流：提交标题，AI包办文案生图，3分钟生成老纪先生风格漫画并发布到公众号草稿箱。

## 工作流概览

提交表单（标题+图片数量）→ AI生成文案 → 生成封面图 → 拆分文案逐条生成正文图 → 合并 → Coze排版发布到公众号草稿箱

## 前置准备

- **通义万相2.5 MCP Server**：需在阿里云百炼开通服务
  - 开通地址：https://bailian.console.aliyun.com/?tab=mcp#/mcp-market/detail/Wan25Media
- **n8n**：工作流自动化平台
- **Coze**：用于排版和发布到公众号草稿箱
- **Gemini 2.5 Pro**：用于文案生成
- **OpenAI/DeepSeek**：用于Chat Model

## 节点拆解

### 1. 开始节点
- 类型：On form submission（提交表单后启动）
- 表单字段：文章标题、图片数量

### 2. 生成文案（AI Agent）
- 基于 Gemini 2.5 Pro，根据标题生成老纪先生漫画风格的文案列表

### 3. 生成封面图

#### 3.1 生成封面图（AI Agent）
- 取文案列表第一条生成封面
- Chat Model：OpenAI Chat Model 或 DeepSeek Chat Model
- Tool：MCP Client Tool（图片生成）
  - Endpoint：`https://dashscope.aliyuncs.com/api/v1/mcps/Wan25Media/sse`
  - Server Transport：Server Sent Events (Deprecated)
  - Authentication：Header Auth
    - Name：`Authorization`
    - Value：`Bearer <阿里云百炼key>`（注意Bearer后有空格）

#### 3.2 Edit Fields（重命名封面字段）
- 将封面图字段名重命名为 `fm`

### 4. 生成正文

#### 4.1 Split Out
- 把文案列表拆分，依次遍历

#### 4.2 生成正文（AI Agent）
- 基于前置文案逐条生成图片，配置同封面图生成

#### 4.3 Code in JavaScript
- 将所有图片链接放入数组

### 5. 发布到草稿箱

#### 5.1 Merge
- 合并封面图和正文漫画
- Mode：Combine
- Combine By：Position
- Inputs：2

#### 5.2 HTTP Request（调用Coze工作流）
- Method：POST
- URL：`https://api.coze.cn/v1/workflow/run`
- Authentication：Header Auth（Coze凭证）
- Headers：`Content-Type: application/json`
- Body（JSON）：
```json
{
    "workflow_id": "7575075753932685318",
    "parameters": {
        "laoji_imgs": {{ $json.allLinks.toJsonString() }},
        "title": "{{ $('On form submission').item.json['文章标题'] }}",
        "wenan": {{ $('生成文案').item.json.output.toJsonString() }},
        "fm": {{ $json.fm.toJsonString() }}
    }
}
```

### Coze 工作流
1. 接收文章标题、文案、封面图、正文图
2. 循环组装图片与文案生成老纪先生风格漫画
3. 通过插件发布到公众号草稿箱

## 关键技术点

- **MCP Client Tool**：通过通义万相2.5的MCP Server实现图片生成，无需自己写API调用
- **Split Out + 遍历**：文案列表拆分后逐条生成图片，实现批量处理
- **n8n + Coze 分工**：n8n负责编排和生图，Coze负责排版和公众号发布
- **表单触发**：On form submission，用户填写标题即可启动全自动流程

## 与前作对比

作者此前发布过养生美食漫画工作流（Coze+n8n实战：养生美食漫画自动化流水线），本文是升级版——从养生主题扩展到老纪先生风格漫画，生图引擎从Coze内置切换到通义万相2.5 MCP Server。
