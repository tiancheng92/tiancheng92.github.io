---
layout: article
title: tiancheng92/gin_example_with_generic模型使用说明
---

# 项目简介

* gin_example_with_generic是一个基于Gin开发的一个简单的API框架，并对controller、service、model层进行泛型化。
* 项目地址：[tiancheng92/gin_example_with_generic](https://github.com/tiancheng92/gin_example_with_generic)

<!--more-->

# 使用方法

## 必要组件

* `mysql`{:.info} >= `5.7`

## 安装

```shell
go get -u github.com/tiancheng92/gin_example_with_generic
```

## 配置

* 修改`./config_file/local.yaml`文件

```yaml
Mysql:
  DBHost: "127.0.0.1"     # 数据库地址
  DBPort: "3306"          # 数据库端口
  DBName: "gin_example"   # 数据库名称
  DBUser: "root"          # 数据库用户名
  DBPassword: ""          # 数据库密码

Server:
  Mode: "debug"           # 运行模式，test、debug或release
  ServicePort: "8080"     # 服务端口
  ServiceHost: "0.0.0.0"  # 服务器地址

Log:
  Level: "debug"          # 日志级别，debug、info、warn、error

I18n:
  Locale: "zh"            # 国际化，zh en ja
```

## 运行

```shell
cd $GOPATH/src/gin-example-with-generic
go run -tags=go_json -gcflags=-l=4 -ldflags=-s -w ./cmd/cmd.go
```

### 项目的运行信息和已存在的API

```text
2022/06/18 12:51:24 maxprocs: Leaving GOMAXPROCS=8: CPU quota undefined
[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:   export GIN_MODE=release
 - using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /debug/pprof/             --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/cmdline      --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/profile      --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] POST   /debug/pprof/symbol       --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/symbol       --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/trace        --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/allocs       --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/block        --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/goroutine    --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/heap         --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/mutex        --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /debug/pprof/threadcreate --> github.com/gin-contrib/pprof.pprofHandler.func1 (2 handlers)
[GIN-debug] GET    /healthz                  --> gin_example_with_generic/controller/api/universal.HealthCheck (5 handlers)
[GIN-debug] GET    /metrics                  --> github.com/gin-gonic/gin.WrapH.func1 (5 handlers)
[GIN-debug] GET    /api/v1/country/:pk       --> gin_example_with_generic/controller/api/v1.CountryInterface.Get-fm (5 handlers)
[GIN-debug] GET    /api/v1/country           --> gin_example_with_generic/controller/api/v1.CountryInterface.List-fm (5 handlers)
[GIN-debug] POST   /api/v1/country           --> gin_example_with_generic/controller/api/v1.CountryInterface.Create-fm (5 handlers)
[GIN-debug] PUT    /api/v1/country/:pk       --> gin_example_with_generic/controller/api/v1.CountryInterface.Update-fm (5 handlers)
[GIN-debug] DELETE /api/v1/country/:pk       --> gin_example_with_generic/controller/api/v1.CountryInterface.Delete-fm (5 handlers)
[GIN-debug] GET    /api/v1/user/:pk          --> gin_example_with_generic/controller/api/v1.UserInterface.Get-fm (5 handlers)
[GIN-debug] GET    /api/v1/user              --> gin_example_with_generic/controller/api/v1.UserInterface.List-fm (5 handlers)
[GIN-debug] GET    /api/v1/user/name/:name   --> gin_example_with_generic/controller/api/v1.UserInterface.ListByName-fm (5 handlers)
[GIN-debug] POST   /api/v1/user              --> gin_example_with_generic/controller/api/v1.UserInterface.Create-fm (5 handlers)
[GIN-debug] PUT    /api/v1/user/:pk          --> gin_example_with_generic/controller/api/v1.UserInterface.Update-fm (5 handlers)
[GIN-debug] DELETE /api/v1/user/:pk          --> gin_example_with_generic/controller/api/v1.UserInterface.Delete-fm (5 handlers)
```

# 项目目录结构说明

```text
├── cmd                             // 程序入口
├── config                          // 读取配置文件方法
├── config_file                     // 配置文件
├── controller                      // 控制器层
│   └── api                         // 控制器层的api
│       ├── universal               // 通用api（404、健康检测等接口）
│       └── v1                      // 具体的api
├── generic                         // 泛型实现
│   ├── controller.go               // 控制器层的泛型实现
│   ├── model.go                    // 重写gorm的Model结构体（为了实现特定方法）
│   ├── paginate.go                 // 分页器
│   ├── repository.go               // 数据库操作的泛型实现
│   ├── request.go                  // 请求体的接口
│   └── service.go                  // service层的泛型实现
├── pkg                             // 通用包
│   ├── ecode                       // 错误码
│   ├── errors                      // 重写pkg/errors包（结合错误码）
│   ├── http                        // 对Gin的一些方法的封装
│   │   ├── bind                    // 数据绑定
│   │   ├── middleware              // 中间件
│   │   │   ├── cross_domain        // 跨域
│   │   │   ├── handle_error        // 统一的错误处理
│   │   │   └── logging             // 日志（access log）
│   │   └── render                  // 数据渲染
│   ├── json                        // json序列化（fast）
│   ├── log                         // 日志（app log）
│   ├── mysql                       // mysql连接函数
│   └── validator                   // 参数校验
├── router                          // 路由
├── server                          // 服务
├── service                         // 服务层
├── store                           // 数据层
│   ├── db_engine.go                // 数据库引擎
│   ├── model                       // 模型
│   └── repository                  // 数据库操作
├── tools                           // 第三方工具（ecode生成）
└── types                           // 类型
    ├── config                      // 配置文件结构体
    ├── const.go                    // 常量 
    ├── paginate                    // 分页结构体
    ├── request                     // 入参结构体
    └── result                      // 返回结构体
```

待补充...
{:.error}
