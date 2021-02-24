# 服务器、路由、端点（Endpoint）

> [02-server_routes_endpoints/00-intro.md](https://github.com/http-rs/tide-book/blob/main/src/02-server_routes_endpoints/00-intro.md)
> <br />
> commit - 759c39d68f63e06b83bedd812f2a4a3a770879f9 - 2021.02.17

Tide 应用程序的中枢部分是服务器结构体 `Server`。通过创建`服务器（Server）`，并对其配置`路由（Route）`和`端点（Endpoint）`，即可启动 Tide 应用程序。

当`服务器（Server）`启动时，通过对传入`请求（Request）` 的 URL 和路由进行匹配，来处理传入`请求（Request）`。匹配路由的请求，随后将被调度到相应的`端点（Endpoint）`。
