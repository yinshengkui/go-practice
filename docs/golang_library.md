# Golang好用的库

## Goxc## 

[https://github.com/laher/goxc](https://github.com/laher/goxc) 

交叉编译，可以一次性编译出16个平台的二进制文件

[https://mozillazg.com/2015/02/go-use-goxc-to-cross-compile.html](https://mozillazg.com/2015/02/go-use-goxc-to-cross-compile.html)

## Simple-json## 

[https://github.com/bitly/go-simplejson](https://github.com/bitly/go-simplejson)

json解析库

## Cobra## 

[https://github.com/spf13/cobra](https://github.com/spf13/cobra)

命令行工具代码生成器

## Viper## 

[https://github.com/spf13/viper](https://github.com/spf13/viper)

配置文件读取库，支持json、toml、yaml、java property config等

## Mux## 

[https://github.com/gorilla/mux](https://github.com/gorilla/mux)

强大的URL路由和dispatcher

## Set## 

[https://github.com/fatih/set](https://github.com/fatih/set)

Golang的set数据结构，补足golang原生数据结构的不足

## Grumpy## 

[https://github.com/google/grumpy](https://github.com/google/grumpy)

将python转换为go语言运行时

## Godep## 

[https://github.com/tools/godep](https://github.com/tools/godep)

go语言依赖解析工具

## Hugo## 

[https://github.com/spf13/hugo](https://github.com/spf13/hugo)

静态网站生成器

## go-dockerclient## 

[https://github.com/fsouza/go-dockerclient](https://github.com/fsouza/go-dockerclient)

支持docker1.11版本的docker客户端，比docker原生客户端包好用

## Burrow## 

[https://github.com/linkedin/Burrow](https://github.com/linkedin/Burrow)

kafka消费者滞后检查

## Advanced-ssh-config## 

[https://github.com/moul/advanced-ssh-config](https://github.com/moul/advanced-ssh-config)

高级SSH工具

## Logrus## 

[https://github.com/Sirupsen/logrus](https://github.com/Sirupsen/logrus)

日志工具

## Nomad## 

这不是一个Golang的库，这是一个基于go语言开发的容器编排工具

[https://github.com/hashicorp/nomad](https://github.com/hashicorp/nomad)

## Codec## 

A High Performance and Feature-Rich Idiomatic encode/decode and rpc library

[https://github.com/ugorji/go/codec](undefined)

## mapstructure## 

将Map转换成Go原生的数据结构

[https://github.com/mitchellh/mapstructure](https://github.com/mitchellh/mapstructure)

## Hoverfly## 

Hoverfly是一个轻量的API服务模拟工具（有时候也被称作[服务虚拟化工具](http://www.infoq.com/cn/news/2013/05/Service-Virtualization)）。 使用Hoverfly，您可以创建应用程序依赖的API的真实模拟。

https://github.com/SpectoLabs/hoverfly

- 创建可重复使用的虚拟服务，在CI环境中替代缓慢和不稳定的外部或第三方服务
- 模拟网络延迟，随机故障或速率限制以测试边缘情况
- 使用多种编程语言扩展和自定义， 包括Go，Java，Javascript，Python
- 导出，共享，编辑和导入API模拟数据
- 提供方便易用的命令行界面hoverctl
- [Java](https://github.com/SpectoLabs/hoverfly-java)和[Python](https://github.com/SpectoLabs/hoverpy)的语言绑定
- REST API
- 使用Go编写，轻巧，高性能，可在任何地方运行
- 提供多种运行模式，可以对HTTP响应进行记录，回放，修改或合成。

Hoverfly由[SpectoLabs](http://specto.io/)开发和维护。

## Centrifugo## 

Centrifugo 是一个用 Golang 实现的基于 [Websocket](https://www.oschina.net/p/websocket) 或者 [SockJS](https://www.oschina.net/p/sockjs) 的实时通信平台。

https://github.com/centrifugal/centrifugo

- 支持数千个同时连接，提供基于频道的出版/订阅模式。PUB/SUB
- 容易和现有系统集成– 不改变已有后端情况下为系统提供实时通信能力。
- HTTP API 和已有后端通信 . API clients for Python, Ruby, PHP, Go, NodeJS.
- 浏览器可以通过SockJS或者纯粹Websocket协议和centrifugal通信. 提供 iOS和Android平台SDK
- 采用Redis实现分布式部署.
- SHA-256 HMAC连接认证和隐私保护
- 多种类型的频道 – 私有, 用户限制，客户端限制
- 通过名字空间灵活配置频道
- 支持即时消息和历史消息
- 支持用户加入/离开消息
- 网络重连后可以恢复消息
- 内置管理界面，提供多种计量(Metrics)
- 可用于WebRTC信令服务器
- 多种部署手段(docker 镜像, RPM/DEB 包, Nginx 配置, TLS certificates)
- MIT license

通讯模型：

![img](https://static.oschina.net/uploads/img/201702/12120949_CfbS.png)

Centrifugo 包含如下子项目：

- [centrifugo](https://github.com/centrifugal/centrifugo) - 采用 Go 语言开发的实时消息传递服务器
- [centrifuge-js](https://github.com/centrifugal/centrifuge-js) - Javascript 客户端，可直接在浏览器使用
- [centrifuge-android](https://github.com/centrifugal/centrifuge-android) - Android 的客户端开发包，可通过 WebSockets 与服务器通讯
- [centrifuge-ios](https://github.com/centrifugal/centrifuge-ios) - Swift 开发包
- [centrifuge-go](https://github.com/centrifugal/centrifuge-go) - Go 客户端开发包
- [cent](https://github.com/centrifugal/cent) - Python 开发包
- [adjacent](https://github.com/centrifugal/adjacent) - Cent 的小型封装包，简化了与 Django 框架的集成
- [rubycent](https://github.com/centrifugal/rubycent) - Ruby gem to communicate  with Centrifugo server API.
- [phpcent](https://github.com/centrifugal/phpcent) - PHP client to communicate  with Centrifugo server API.
- [gocent](https://github.com/centrifugal/gocent) - Go client to communicate  with Centrifugo server API.
- [jscent](https://github.com/centrifugal/jscent) - NodeJS client to communicate  with Centrifugo server API.
- [web](https://github.com/centrifugal/web) - Centrifugo 的管理界面，基于 [ReactJS](https://www.oschina.net/p/facebook-react) 开发

**goconvey**

https://github.com/smartystreets/goconvey

GO语言测试，Go testing in the browser. Integrates with `go test`. Write behavioral tests in Go. [http://goconvey.co](http://goconvey.co/)

**open-golang**

https://github.com/skratchdot/open-golang/

使用操作系统的默认启动程序打开文件、目录或URI，也可以指定一个应用打开。

**zap**

https://github.com/uber-go/zap

uber开源的高性能结构化日志工具