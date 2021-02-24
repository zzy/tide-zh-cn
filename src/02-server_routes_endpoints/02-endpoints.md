# 使用端点（Endpoint）处理请求

> [02-server_routes_endpoints/02-endpoints.md](https://github.com/http-rs/tide-book/blob/main/src/02-server_routes_endpoints/02-endpoints.md)
> <br />
> commit - 759c39d68f63e06b83bedd812f2a4a3a770879f9 - 2021.02.17

为了使`服务器（Server）`返回 HTTP 404 回应以外的任何内容，我们需要告诉服务器如何对请求作出反应。我们通过添加一个或多个端点（Endpoint）来实现；

```rust,edition2018,no_run
#[async_std::main]
async fn main() -> tide::Result<()> {
    let mut server = tide::new();
    server.at("*").get(|_| async { Ok("Hello, world!") });
    server.listen("127.0.0.1:8080").await?;
    Ok(())
}
```

我们使用 `at` 方法指定到达端点（Endpoint）的路由（我们稍后再讨论路由）。目前，我们仅使用 `"*"` 通配符路由，它与任何路径都可以匹配。示例中，我们添加了一个异步闭包作为`端点（Endpoint）`。Tide 期望路由后添加的`端点（Endpoint）`函数的参数都实现 `Endpoint` trait。但当前实例中的闭包也是有效的，因为 Tide 使用如下签名实现了某些异步函数的 `Endpoint` trait；

```rust,ignore
async fn endpoint(request: tide::Request) -> tide::Result<impl Into<Response>>
```

如上示例中，`&str` 实现了 `Into<Response>`，因此我们的闭包是一个有效的端点（Endpoint）。这是因为，`Into<Response>` 是为其它几种可以快捷设定端点（Endpoint）的类型实现的。例如，下述实例中，端点（Endpoint）使用由 `use tide::prelude::*` 提供的 `json!` 宏，返回 `serde_json::Value`。

```rust,edition2018,no_run
use tide::prelude::*;
#[async_std::main]
async fn main() -> tide::Result<()> {
    let mut server = tide::new();
    server.at("*").get(|_| async {
        Ok(json!({
            "meta": { "count": 2 },
            "animals": [
                { "type": "cat", "name": "chashu" },
                { "type": "cat", "name": "nori" }
            ]
        }))
    });
    server.listen("127.0.0.1:8080").await?;
    Ok(())
}
```

示例中，响应会返回字符串或 json 结果，对于获得能运作的端点（Endpoint）而言，这些用法很便捷。下面示例中，我们返回完整的响应结构体 `Response`。

```rust,ignore
server.at("*").get(|_| async {
    Ok(Response::new(StatusCode::Ok).set_body("Hello world".into()))
});
```

下一章中，将更详细地描述 `Response` 类型。

可以通过方法链添加多个端点（Endpoint）。例如，如果我们想回应一个 `delete` 请求以及一个 `get` 请求，那么可以为两者各自添加端点（Endpoint）；

```rust,ignore
server.at("*")
    .get(|_| async { Ok("Hello, world!") })
    .delete(|_| async { Ok("Goodbye, cruel world!") });
```

最后，当我们的端点（Endpoint）方法一点点增长时，路由的定义将变得拥挤。我们可以将端点（Endpoint）实现转移到它们自己的函数中；

```rust,edition2018,no_run
#[async_std::main]
async fn main() -> tide::Result<()> {
    let mut server = tide::new();
    server.at("*").get(endpoint);
    server.listen("127.0.0.1:8080").await?;
    Ok(())
}

async fn endpoint(_req: tide::Request<()>) -> Result<Response> {
    Ok(Response::new(StatusCode::Ok).set_body("Hello world".into()))
}
```
