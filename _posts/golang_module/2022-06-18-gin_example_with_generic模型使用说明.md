---
layout: article
title: gin_example_with_generic模型使用说明
tags: [golang, module]
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

### 项目的运行信息

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

# 项目目录结构

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

# 各个模块说明

## 泛型实现

### Model层

* 数据库模型需要实现[ModelInterface](https://github.com/tiancheng92/gin_example_with_generic/blob/main/generic/model.go)
  接口，自带的`SoftDeleteModel`与`Model`默认实现了`GetPrimaryKeyName() string`方法与`GetPrimaryKey() any`
  方法，即用户若在模型结构体中嵌套了`SoftDeleteModel`或`Model`，模型结构体仅需实现`GetFuzzySearchFieldList() []string`即可。
* ModelInterface详解：

```golang
type ModelInterface interface {
    GetPrimaryKeyName() string          // 获取主键名称
    GetPrimaryKey() any                 // 获取主键值
    GetFuzzySearchFieldList() []string  // 获取允许模糊查询的字段列表
}
```

* Model层泛型结构体`Repository[M ModelInterface]`默认实现了
  [RepositoryInterface](https://github.com/tiancheng92/gin_example_with_generic/blob/main/generic/repository.go)
  接口，用户只需在自己的Repository层继承`Repository[M ModelInterface]`结构体，即可实现默认的对数据库增删改查操作。
* RepositoryInterface详解：

```golang
type RepositoryInterface[M ModelInterface] interface {
	Get(ctx context.Context, pk any) (*M, error)                        // 查询指定主键的数据
	Create(ctx context.Context, attributes M) (*M, error)               // 创建数据
	Update(ctx context.Context, pk any, attributes M) (*M, error)       // 更新数据
	Delete(ctx context.Context, pk any) error                           // 删除数据
	List(ctx context.Context, pq *paginate.Query) (*Paginate[M], error) // 分页查询
}
```

* 用户可在自己的Repository层继承`Repository[M ModelInterface]`结构体后对`Repository[M ModelInterface]`
  中的方法进行重写或新增，参考[store/repository/user.go](https://github.com/tiancheng92/gin_example_with_generic/blob/main/store/repository/user.go)
  。

### Service层

* Service层泛型包含了俩个结构体模型：
    * `ReadOnlyService[M ModelInterface]`
      只读服务层，实现了[ReadOnlyServiceInterface](https://github.com/tiancheng92/gin_example_with_generic/blob/main/generic/service.go)
      接口
    * `Service[R RequestInterface, M ModelInterface]` 默认服务层,
      实现了[ServiceInterface](https://github.com/tiancheng92/gin_example_with_generic/blob/main/generic/service.go)接口
* 用户只需在自己的Service层继承`ReadOnlyService[M ModelInterface]`或`Service[R RequestInterface, M ModelInterface]`
  结构体，即可实现对Service层增删改查操作。
* 泛型Service仅仅实现了Controller层数据的拓传以及把请求数据格式化为数据库模型操作。
* 泛型Service接口详解：

```golang
type ServiceInterface[R RequestInterface, M ModelInterface] interface {
	Get(ctx context.Context, pk any) (*M, error)                         // 查询指定主键的数据
	List(ctx context.Context, pq *paginate.Query) (*Paginate[M], error)  // 分页查询
	Update(ctx context.Context, pk any, request *R) (*M, error)          // 更新数据
	Create(ctx context.Context, request *R) (*M, error)                  // 创建数据
	Delete(ctx context.Context, pk any) error                            // 删除数据
}

type ReadOnlyServiceInterface[M ModelInterface] interface {
	Get(ctx context.Context, pk any) (*M, error)                         // 查询指定主键的数据
	List(ctx context.Context, pq *paginate.Query) (*Paginate[M], error)  // 分页查询
}
```

* 用户可在自己的Service层继承`ReadOnlyService[M ModelInterface]`或`Service[R RequestInterface, M ModelInterface]`
  结构体后对接口中的方法进行重写或新增，参考[store/service/user.go](https://github.com/tiancheng92/gin_example_with_generic/blob/main/service/user.go)
  。

### Controller层

* 从API接口获取的数据模型需要实现
  [RequestInterface](https://github.com/tiancheng92/gin_example_with_generic/blob/main/generic/request.go)接口。
* RequestInterface详解：

```golang
type RequestInterface interface {
	FormatToModel() ModelInterface  // 将请求数据格式化为数据库模型
}
```

* Controller层泛型包含了俩个结构体模型：
    * `ReadOnlyController[M ModelInterface]`
      只读Controller层，实现了[ReadOnlyControllerInterface](https://github.com/tiancheng92/gin_example_with_generic/blob/main/generic/controller.go)
      接口
    * `Controller[R RequestInterface, M ModelInterface]` 默认Controller层,
      实现了[ControllerInterface](https://github.com/tiancheng92/gin_example_with_generic/blob/main/generic/controller.go)
      接口
* 用户只需在自己的Controller层继承`ReadOnlyController[M ModelInterface]`或`Controller[R RequestInterface, M ModelInterface]`
  结构体，即可实现对Controller层增删改查操作。
* 泛型Controller仅仅实现了默认的数据绑定以及向Service层传递数据的操作。
* 泛型Controller接口详解：

```golang
type ControllerInterface[R RequestInterface, M ModelInterface] interface {
	Get(ctx *gin.Context)    // 查询指定主键的数据
	List(ctx *gin.Context)   // 分页查询
	Update(ctx *gin.Context) // 更新数据
	Create(ctx *gin.Context) // 创建数据
	Delete(ctx *gin.Context) // 删除数据
}

type ReadOnlyControllerInterface[M ModelInterface] interface {
	Get(ctx *gin.Context)   // 查询指定主键的数据
	List(ctx *gin.Context)  // 分页查询
}
```

* 用户可在自己的Controller层继承`ReadOnlyController[M ModelInterface]`或`Controller[R RequestInterface, M ModelInterface]`
  结构体后对接口中的方法进行重写或新增，参考[controller/api/v1/user.go](https://github.com/tiancheng92/gin_example_with_generic/blob/main/controller/api/v1/user.go)
  。

## 分页

待补充...
{:.error}

## 参数绑定

待补充...
{:.error}

## 参数校验

待补充...
{:.error}

## 数据渲染

待补充...
{:.error}

## 中间件

待补充...
{:.error}

## 错误处理以及错误码

待补充...
{:.error}

# 构建相关

模型内建了自己的json包，支持使用[go_json](https://github.com/goccy/go-json)或[json-iterator](https://github.com/json-iterator/go)
进行json解析。
{:.info}

| json解析方式                    | 构建约束                        |
|-----------------------------|-----------------------------|
| encoding/json               | go build ...                |
| github.com/goccy/go-json    | go build -tags=go_json ...  |
| github.com/json-iterator/go | go build -tags=jsoniter ... |

