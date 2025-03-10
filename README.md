# optima 服务端 - AI 聊天、协作、图像生成

一款集成了主流大语言模型以及绘图模型的 APP 服务端，使用 Golang 开发，代码完全开源，支持以下功能：

- 支持 GPT-3.5/4 问答聊天
- 支持国产模型：通义千问，文心一言、讯飞星火
- 支持开源模型：Llama2 ，ChatGLM2 ，AquilaChat 7B ，Bloomz 7B 等，后续还将开放更多
- 支持文生图、图生图、超分辨率、黑白图片上色等功能，集成 Stable Diffusion 模型，支持 SDXL 1.0

开源代码：

- 客户端：https://github.com/mylxsw/aidea
- 服务端：https://github.com/mylxsw/aidea-server

### 技术交流

- 微信技术交流群：3 个群都已满员，添加微信号 `x-prometheus` 为好友，拉你进群

    <img src="https://github.com/mylxsw/aidea/assets/2330911/655601c1-9371-4460-9657-c58521260336" width="200"/>

- 微信公众号

    <img src="https://github.com/mylxsw/aidea-server/assets/2330911/376a3b9f-eacd-45c6-9630-39eb720ba097" width="500" />

- 电报群：[点此加入](https://t.me/aideachat)

## APP 下载安装地址

Android/IOS APP：https://aidea.aicode.cc/

> IOS 国区目前不可用，除此之外所有区域都可以下载。
>
> 2023 年 9 月 4 日发现国区被下架后，非国区 IOS 版本可能无法完成应用内购（提示“你的购买无法完成”，但是也有人能够成功支付），暂时无法充值，最新进展在这里 [issue#16](https://github.com/mylxsw/aidea/issues/16)。

Mac/Windows 桌面端：https://github.com/mylxsw/aidea/releases

Web 端：https://web.aicode.cc/

## 福利

目前我的 OpenAI 账户还有大约 4900+ 美金的额度，为了感谢各位的关注，在 2023 年 11 月 1 日前（4900 美金额度消耗完为止），GPT-4 使用价格低至 每 1K Token 只需要 10 个智慧果。

> 相当于 0.1 元 / 1000 Token，OpenAI 官方价格为输入 0.21 元，输出 0.42 元。

## 关于代码

>  目前代码注释、技术文档还比较少，后续有时间会进行补充，敬请见谅。另外以下几点请大家注意，以免造成困扰：
>
> - 代码中 `Room`，`顾问团` 均代表 `数字人`，因项目经过多次改版和迭代，经历了 `房间` -> `顾问团` -> `数字人` 的名称调整
> - 代码中 v1 版本的 `创作岛` 与 v2 版本截然不同，其中 v1 版本服务于 App 1.0.1 及之前版本，从 1.0.2 开始，这部分不再使用，所以就有了
    v2 版本

项目所用的框架

- [Glacier Framework](https://github.com/mylxsw/glacier)： 自研的一款支持依赖注入的模块化的应用开发框架，它以 [go-ioc](https://github.com/mylxsw/go-ioc) 依赖注入容器核心，为 Go 应用开发解决了依赖传递和模块化的问题
- [Eloquent ORM](https://github.com/mylxsw/eloquent) 自研的一款基于代码生成的数据库 ORM 框架，它的设计灵感来源于著名的 PHP 开发框架 Laravel，支持 MySQL 等数据库

代码结构如下

| 目录 | 说明                                                                        |
| --- |---------------------------------------------------------------------------|
| api | 对外公开的 API 接口，控制器在这里实现                                                     |
| config | 配置定义、管理                                                                   |
| migrate | 数据库迁移文件，SQL 文件 |
| internal/ai | 不同厂商的 AI 模型接口实现                                                           |
| internal/ai/chat | 聊天模型抽象接口，所有聊天模型都在这里封装为兼容 OpenAI Chat Stream 协议的实现                         |
| internal/aliyun | 阿里云短信、内容安全服务实现                                                            |
| internal/coins | 服务定价、收费策略                                                                 |
| internal/dingding | 钉钉通知机器人                                                                   |
| internal/helper | 部分助手函数                                                                    |
| internal/jobs | 定时任务，用户每日智慧果消耗额度统计等                                                       |
| internal/mail | 邮件发送                                                                      |
| internal/payment | 在线支付服务实现，如支付宝，Apple                                                       |
| internal/proxy | Socks5 代理实现                                                               |
| internal/queue | 任务队列实现，所有异步处理的任务都在这里定义                                                    |
| internal/queue/consumer | 任务队列消费者                                                                   |
| internal/rate | 流控实现                                                                      |
| internal/redis | Redis 实例                                                                  |
| internal/repo | 数据模型层，封装了对数据库的操作                                                          |
| internal/repo/model | 数据模型定义，使用了 [mylxsw/eloquent](https://github.com/mylxsw/eloquent)  来创建数据模型 |
| internal/service | Service 层，部分不适合放在 Controller 和 Repo 层的代码，在这里进行封装 |
| internal/sms | 统一的短信服务封装，对上层业务屏蔽了底层的短信服务商实现 |
| internal/tencent | 腾讯语音转文本、短信服务实现 |
| internal/token | JWT Token |
| internal/uploader | 基于七牛云存储实现的文件上传下载 |
| internal/voice |  基于七牛云的文本转语音实现，暂时未启用 |
| internal/youdao | 有道翻译服务 API 实现 |
| config.yaml | 配置文件示例 |
| nginx.conf | Nginx 配置示例 |
| systemd.service | Systemd 服务配置示例 |

项目编译：

```bash
go build -o build/debug/aidea-server cmd/main.go
```

## 服务端部署

如果你不想使用托管的云服务，可以自己部署服务端，[部署请看这里](./docs/deploy.md)。

## 如果对你有帮助，请我喝杯酒吧

微信 | 支付宝
:----------:|:---------:
 ![image](https://github.com/mylxsw/aidea/assets/2330911/46e2242b-17bc-41ff-bebe-b5cc466b7f17) | ![image](https://github.com/mylxsw/aidea/assets/2330911/f3c85d4a-bea8-4a76-b582-c673613f76cb)

## Star History

<a href="https://star-history.com/#mylxsw/aidea-server">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=mylxsw/aidea-server&type=Date&theme=dark" />
    <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=mylxsw/aidea-server&type=Date" />
    <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=mylxsw/aidea-server&type=Date" />
  </picture>
</a>

## License

MIT

Copyright (c) 2023, mylxsw
