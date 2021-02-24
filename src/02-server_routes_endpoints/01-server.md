# 构建服务器

> [02-server_routes_endpoints/01-server.md](https://github.com/http-rs/tide-book/blob/main/src/02-server_routes_endpoints/01-server.md)
> <br />
> commit - 759c39d68f63e06b83bedd812f2a4a3a770879f9 - 2021.02.17

使用 `tide::new()` 方法构建基本的 Tide `服务器（Server）`。

```rust,edition2018,no_run
#[async_std::main]
async fn main() -> tide::Result<()> {
    let server = tide::new();
    Ok(())
}
```

然后，可以使用异步的 `listen` 方法启动服务器。

```rust,edition2018,no_run
#[async_std::main]
async fn main() -> tide::Result<()> {
    let server = tide::new();
    server.listen("127.0.0.1:8080").await?;
    Ok(())
}
```

虽然这是你所能构建的最简单的 Tide 应用程序，但它并不是很有用。它将对任何请求都返回 404 HTTP 响应。为了能够返回任何有用的信息，我们需要使用一个或多个`端点（Endpoint）`来处理请求。
