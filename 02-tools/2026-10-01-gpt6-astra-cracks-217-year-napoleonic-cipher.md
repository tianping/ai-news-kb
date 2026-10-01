# 尘封217年的拿破仑密信，被GPT-6 Astra用6小时解开

> 收藏自微信公众号 · 2026-10-01 · https://mp.weixin.qq.com/s/qfN1FxnrKK9b9QktWr_UDg

## 核心事件

- **人物**：美国 AI 工程师 Carter Church（SentinelOne AI 工程技术人员，网络安全/事件响应背景），2026-09-30 在社交平台公布
- **成果**：借助 OpenAI GPT-6 Astra 破解一封 1809 年写给法国将领奥古斯特·德·马尔蒙（Auguste de Marmont）的加密军事简报，模型执行约 6 小时
- **材料**：唯一公开的完整图像来自法国军事史期刊《Revue historique des Armées》1969 年刊载的扫描图（1202×1836 像素，低清黑白），原图长期躺在 Cryptiana 未破解密码清单中
- **验证**：Cryptiana 维护者 Satoshi Tomokiyo 检查解法后标记为"已解决"，并更正此前标错的年份 1807 → 1809；Church 9/18 已公布完整研究记录（转录文本、密码表、复现程序）

## 信件背景

- 写信方：意大利总督欧仁·德·博阿尔内（Eugène de Beauharnais）司令部，按拿破仑命令转述的加密军事简报（非拿破仑亲笔直接寄出）
- 时间重定为 1809 年 3 月下旬，距奥地利发动进攻约两周
- 解密内容：约 4 万巴伐利亚军驻慕尼黑/帕绍、约 3 万波兰军驻维斯瓦河威胁克拉科夫、萨克森军驻德累斯顿；达武/马塞纳/乌迪诺所部位置与兵力；与拿破仑 3/16 给欧仁的命令逐项对应
- 趣闻：1865 年《拿破仑书信集》相关命令中的省略号"一小撮……"，解出的接收端密信对应位置为"quelques troupes ou un rassemblement de canailles"（几支部队，或一群乌合之众）——但这是转述整理版，不能确认是拿破仑原话

## 密码学特点

- 约 1,300 个密码单元、155 种符号；高频字母多个写法 + 部分符号代表整词（同音替换密码，刻意打散字母频率）
- 此前 Daniel Tant 已整理出 33 个字母对应关系（可解释 435/1,300 单元，约 1/3）
- 关键发现：1969 年原始期刊已被 Persée 数字化，所需材料其实一直公开存在，只是藏在难用的老扫描图里

## Astra 智能体工作流（6 小时执行内容）

1. **转录**：低分辨率扫描图切行识别手写符号 → 初记 1320 单元/175 符号 → 合并变体后 1300 单元/155 符号
2. **锚点**：以 Tant 的 33 个字母值为锚，利用信中明写"CONSEQUENT"等扩大已知部分
3. **求解**：模拟退火算法程序 + 19 世纪法语语料（雨果、大仲马、马尔蒙回忆录）判断自然度；发现仅按单字母解释不通 → 识别出 29 个整词符号（de、que、les、vous、général 等）
4. **验证**：每个新符号映射回原图检查其他出现位置；移除马尔蒙回忆录与拿破仑时代文本重跑仍恢复出相同主要内容
5. 全程无新密码学原理——价值在于把图像转录、史料检索、语言分析、密码破解、程序开发多环节串联压缩

## 同批"AI 破历史密码"案例（外部验证程度不一）

| 案例 | 工具 | 耗时 | 验证状态 |
|------|------|------|---------|
| 1941 年德军 Enigma 电报 MVUEH（Carter Leffen） | GPT-6 Astra | 自行筛选目标+写 Python/C++ 模拟 | ✅ Crypto Cellar 维护者 Frode Weierud 确认明文与密钥 |
| 本信（1809 马尔蒙密信） | GPT-6 Astra | ~6 小时 | ✅ Cryptiana 维护者确认 |
| 1941 年 Enigma 密文 FMNGI（Jack Willis，9/20） | Claude Opus 5（预提供 Go 工具+史料） | 最终一轮搜索 13 分 28 秒（Apple M2） | — |
| 1918 年 ADFGVX 无线电密文 | GPT-6 Astra | — | ⚠️ 存争议：密钥与密文时间不完全对应，缺独立复现 |

## 意义

- AI 降低历史密码学研究成本：材料与方法早已存在，缺的是投入时间；模型把数周工作压缩到更短时间尺度
- 可推广到历史档案、古文字、手稿、旧地图及散落数据库——"未解之谜"清单会越来越短，参与者也可能越来越多来自专业之外
- Church 本人：有耐心的密码专家几十年前也可能解开，只是很少有人愿意为一张军事史期刊里的低清扫描图投入数周

## 参考链接

1. https://carter.church/writeups/the-letter-to-marmont/
2. https://www.persee.fr/doc/rharm_0035-3299_1969_num_25_4_8686
3. https://cryptocellar.org/bgac/g-army-july-1941.html
4. https://cryptocellar.org/bgac/e-keys-july-1941.html
5. https://www.cryptocellar.org/bgac/the-fmngi-break.html
6. https://www.thinkfacility.com/blog/gpt-6-astra-solved-a-1918-adfgvx-radio-message/
