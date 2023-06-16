---
title: 封装属于你的Gin框架（一）：开篇 & 项目初始化
date: 2023-06-16
slug: go
image: go-gin.webp
categories:
    - 后端
tags:
    - golang
    - Gin

---

### 前言

我是一名 phper，由于各方面因素，决定转战 Go，PHP 基本都是用来开发 Web 项目的，所以这次就使用 Go 中最流行的 Web 框架 Gin 来进行二次封装，由于它自由度很高，没办法像 PHP 框架 Laravel 开箱即用，所以就诞生了这个系列的文章，带你一步步将基础服务封装到 Gin 中，方便以后更愉快的 CURD

### 适用人群

- 懂得安装 Go 环境及其基本语法
- 会使用 Go Modules 管理项目
- 略微有一点点点的开发经验

### 目录结构

![image-20211009001616865.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6e4c24c6f39c47908c5295ba01e9c7c7~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

| 文件/目录名称   | 说明                           |
| --------------- | ------------------------------ |
| app/common      | 公共模块（请求、响应结构体等） |
| app/controllers | 业务调度器                     |
| app/middleware  | 中间件                         |
| app/models      | 数据库结构体                   |
| app/services    | 业务层                         |
| bootstrap       | 项目启动初始化                 |
| config          | 配置结构体                     |
| global          | 全局变量                       |
| routes          | 路由定义                       |
| static          | 静态资源（允许外部访问）       |
| storage         | 系统日志、文件等静态资源）     |
| utils           | 工具函数                       |
| config.yaml     | 配置文件                       |
| main.go         | 项目启动文件                   |

### 初始化项目

1. 先在 `~/go/src` 目录下创建一个目录 `jassue-gin` 用来存放项目代码

```shell
mkdir ~/go/src/jassue-gin
```

1. 在项目根目录下，初始化 `go.mod` 文件

```shell
go mod init jassue-gin
```

1. 安装 Gin

```shell
go get -u github.com/gin-gonic/gin
```

1. 在项目根目录下编写 `main.go` 文件

```go
package main

import (
    "github.com/gin-gonic/gin"
    "net/http"
)

func main() {
    r := gin.Default()

    // 测试路由
    r.GET("/ping", func(c *gin.Context) {
        c.String(http.StatusOK, "pong")
    })

    // 启动服务器
    r.Run(":8080")
}
```

### 启动应用 & 测试

执行 `go run main.go` 启动应用

![image-20211009000758685.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/efde8dfda9ed4b1ca2137c7ddb4060bc~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

可以看到 HTTP 服务器启动成功，打开 [http://127.0.0.1:8080/ping](https://link.juejin.cn?target=http%3A%2F%2F127.0.0.1%3A8080%2Fping) 测试路由

![image-20211009001053698.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8a536672108d43148064db16d938087a~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)
