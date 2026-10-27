# Ollama v0.40.9：Mac 上用 Phonon 本地流式语音合成，另有 d3 决策模型

- **日期**：2026-10-26（收藏）
- **来源**：微信公众号「养虾农户」
- **原标题**：Ollama v0.40.9 在 Mac 上开口说话了
- **分类**：02 工具与产品
- **URL**：https://mp.weixin.qq.com/s/zsPDQvnI_OoczGWmZM5TVw

## 概述

本地模型以前只能输出文字，要做语音得把文本发到云端，或者自己接一套本地 TTS。10 月 22 日发布的 Ollama v0.40.9 开始在 Apple Silicon 上支持用 Phonon 生成语音，并且是边生成边朗读：

```bash
ollama run phonon "Hello from Ollama."
```

## Phonon 语音模型

- Gradium 出品的语音合成模型家族，Ollama 库里有 14 个变体
- 体积：英文版 104MB，其他语言 123MB；上下文均为 8K
- 语言：德、英、西、法、葡，另有多语言版；每种语言分 31m、40m 两种规模
- **没有中文档**：多语言版能否读中文需要自己试
- 作者认为体积小是因为语音合成任务窄（输入文字、输出音频帧），参数本来就少。好处是加载快、占内存少，不和几十 GB 的聊天模型抢显存

## 关键特性

- **流式朗读**：`ollama run` 可以直接朗读文本或聊天模型刚生成的回复，音频流式产出，不用等整段合成完。这决定了交互是否成立：等三秒才出声，用户会以为卡了
- **OpenAI 兼容接口**：应用侧走 `/v1/audio/speech`，可流式获取音频；现有 OpenAI 客户端把地址指过来就能用，不改逻辑
- **限制**：目前仅 Apple Silicon，Linux、Windows 和带独显的机器都还没有（基于 MLX 的音频能力先上 Mac 是该项目的惯例）；在 Linux 服务器上跑 agent 的话，这个版本暂无直接收益

## 同版本其他更新

- **d3 决策模型**：移植自 vLLM Semantic Router，分 2B/4B/9B/27B 四档（3.1/5.6/10/29GB），上下文 256K，可读图和文本。不当聊天模型用，而是走 `/v1/systemone` 接口一次问一组问题、返回概率。题型三种：`noul`（是/否）、`choice`（多选一）、`score`（分级打分）

  ```bash
  curl http://localhost:11434/v1/systemone \
    -d '{"model":"d3","state":"Hello World",
         "questions":{"says_hello":{"type":"noul",
           "instructions":"这段文字里有问候吗？",
           "criteria":{"true":"有","false":"没有"}}}}'
  ```

- **Flint** 从仅支持 macOS 扩展到 Linux 和 Windows
- **三项修复**（都是长时间运行才会碰到的）：
  - 流式请求中途出错，以前会静默结束，客户端以为已经说完，现在会报错
  - `ollama launch` 以前不把云端模型的上下文长度告诉编码工具，导致压缩阈值设错，现已修复
  - Anthropic 兼容的联网搜索，后续请求改用模型实际支持的思考设置

## 使用建议（作者）

- Mac 上想先听听效果：`ollama run phonon` 加一句话即可
- 做产品别自己造轮子，直接对接 `/v1/audio/speech`，客户端按 OpenAI 协议写，以后换后端不用改
- 需要中文先试多语言版；不行就等，补语言通常是这类小模型最快的一步

## 备注

- 正文由 fetch_wechat.py 抓取；版本号和功能细节以 Ollama 官方 release notes 为准
- 相关笔记：[2026 最佳 Ollama 模型](2026-08-12-best-ollama-models-2026.md)、[Ollama Modelfile 五种人设](2026-08-29-ollama-modelfile-5-personas.md)
