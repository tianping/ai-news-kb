# REA 逆向热潮：某星电视被曝"监视"用户，REA 遭 Claude / ChatGPT 封禁

- **日期**：2026-10-26（收藏）
- **来源**：微信公众号「JackCui」
- **原标题**：太刑了，某电视被曝监视用户！REA被Claude、ChatGPT集体封禁！
- **分类**：05 事件与评论
- **URL**：https://mp.weixin.qq.com/s/5sGQzYRelElAIitEyfasjA

## 概述

REA 是一套给 AI 用的开源逆向工程工具集（GitHub 约 38.6 万 Star），让 AI 在没有源代码的情况下拆解软件、复刻功能，把逆向门槛降到"会打字就能逆向"。由此掀起逆向家电热潮，博主 Log4j 逆向自家某星电视后晒出疑似监视用户的代码；随后 OpenAI 与 Anthropic 先后对 REA 相关请求收紧——ChatGPT 直接封号，Claude 中途打断回答。

## 一、REA 的出圈玩法

- 小米平板 5 刷入 iOS（"关于本机"显示 iOS 16.5 / iPhone X），可同时装 Android/Windows/Linux/iOS：https://github.com/oosexgang/hackios-xnu-kernel
- 逆向智能马桶：拆解微信小程序蓝牙协议，做原生遥控器
- 逆向牙齿扫描软件并开源；用 TypeScript + Three.js 重写整部《上古卷轴》
- 智能电视去广告：通过 Wi-Fi 停用 Android TV / Google TV 广告服务与预装，换 Apple TV 风格桌面：https://github.com/Gathin23/cleanyourtv
- 逆向 Claude 前端：暗色模式下白字因视网膜光渗效应显粗 10%~15%，故字重从 500/600 降到 460/560，使两种主题观感一致：https://github.com/Caho1/open-claude-ui
- 逆向 Claude Code 本地客户端：提取 2589 个模块、约 47MB 代码，内存吃爆：https://github.com/morluto/rea/issues/1563

## 二、awesome-reverse-engineered 合集

- 收录 16,809 个开源逆向项目：游戏反编译与重实现 6071 个、设备与物联网 3059 个，另有 AI 工具/大模型客户端、汽车、固件、文件格式等分类
- 「起死回生」栏目：130 个被厂商关停/变砖/放弃后被社区救活的产品（Spotify Car Thing、亚马逊 Dash 按钮、谷歌停服的 Nest 一二代恒温器等）
- https://github.com/alexgaoth/awesome-reverse-engineered

## 三、Log4j 对某星电视的发现

1. **突破**：固件 AES 加密无密钥；放弃拆机走 UART/JTAG，改用零日漏洞拿到 root
2. **常驻语音**：`security-policy.xml` 中有 `csapi-tv-service-alwaysonvoice` 组件（"Hi, Bixby" 免按键唤醒，麦克风常开）
3. **声音分类上报**（10-23 发帖）：`EV201/SOUNDSENSOR_ON` 事件下有统计狗叫、婴儿哭声的计数器，以键值对上报云端；计数与账号关联可推断家庭日常
4. **摄像头字段**：遥测中含传感器设置、事件计数、摄像头推流时长、光线情况；另有人脸边界框代码与摄像头预览功能
5. **借用外部麦克风**：电视无麦克风时会寻找回音壁、蓝牙等带麦克风设备，数据送 Bixby。Hugging Face 工程师 ngxson 用 Flipper Zero 伪装 USB 麦克风验证：电视确实打开麦克风，5 秒后停止
6. **广告追踪**：按地区加载神经网络追踪 YouTube 等 App 内行为（含跳过哪些广告）；跨设备识别；`rmdemon` 服务可启动网络抓包
7. **地理定位**：指控向 400+ 数据合作方发送坐标；猜测电视越用越卡与后台处理音频/截图有关

## 四、平台封禁

- 帖子刷屏约 42 小时后，Log4j 的 OpenAI 账号以"网络滥用（Cyber Abuse）"被封，约 500 美元额度清零；他称只在本地逆向，触发了 Daybreak 检测
- 有人仅输入 reverse、crawler 等词几分钟后即被封号
- 10-21 起 Reddit r/Claude 用户反映：Claude Opus 5.5 做逆向分析时正常开始，中途被安全检查打断；提示词含 REA 即被强制中断
- 结果：越来越多人转向本地模型跑 REA 以避免封号

## 影响 / 备注

- 作者观点：REA 的意义在于打破厂商与用户之间的信息不对称，用户可以亲自查看设备在后台做了什么
- 但发现可疑代码 ≠ 厂商一定在监视：代码是否实际执行、数据是否真的上传，仍需更多证据（文中均为 Log4j 单方说法）
- AI 降低逆向门槛与平台安全策略之间的张力才刚开始
- 正文由 fetch_wechat.py 抓取
