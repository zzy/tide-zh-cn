# 服务器、路由、端点（Endpoint）

> [02-server_routes_endpoints.md](https://github.com/http-rs/tide-book/blob/main/src/02-server_routes_endpoints.md)
> <br />
> commit - 32c87443efd06c62d0fab49e93e93605b1a7acb6 - 2020.12.02

Tide 应用程序的中枢部分是服务器结构体 `Server`。通过创建`服务器（Server）`，并对其配置`路由（Route）`和`端点（Endpoint）`，即可启动 Tide 应用程序。当`服务器（Server）`启动时，通过对传入`请求（Request）` 的 URL 和路由进行匹配，来处理传入`请求（Request）`。匹配路由的请求，随后被调度到相应的`端点（Endpoint）`。

## 设置服务器

使用 `tide::new()` 方法构建基本的 Tide `服务器`。

```rust
#[async_std::main]
async fn main() -> tide::Result<()> {
    let server = tide::new();
    Ok(())
}
```

然后，可以使用异步的 `listen` 方法启动服务器。

```rust
#[async_std::main]
async fn main() -> tide::Result<()> {
    let server = tide::new();
    server.listen("127.0.0.1:8080").await?;
    Ok(())
}
```

虽然这是你所能构建的最简单的 Tide 应用程序，但它并不是很有用。它将对任何请求返回 404 HTTP 响应。为了能够返回任何有用的信息，我们需要使用一个或多个`端点（Endpoint）`来处理请求。

## 使用端点（Endpoint）处理请求

为了使`服务器`返回 HTTP 404 回应以外的任何内容，我们需要告诉服务器如何对请求作出反应。我们通过添加一个或多个端点（Endpoint）来实现；

```rust
#[async_std::main]
async fn main() -> tide::Result<()> {
    let mut server = tide::new();
    server.at("*").get(|_| async { Ok("Hello, world!") });
    server.listen("127.0.0.1:8080").await?;
    Ok(())
}
```

我们使用 `at` 方法指定到达端点（Endpoint）的路由（我们稍后再讨论路由）。目前，我们仅使用 `"*"` 通配符路由，它将与我们抛出的任何内容相匹配。示例中，我们添加了一个异步闭包作为`端点（Endpoint）`。Tide 期望路由后添加的`端点（Endpoint）`函数的参数都实现 `Endpoint` trait。但实例中的闭包是有效的，因为 Tide 使用如下签名实现了某些异步函数的 `Endpoint` trait；

```rust
async fn endpoint(request: tide::Request) -> tide::Result<impl Into<Response>>
```

如上示例中，`&str` 实现了 `Into<Response>`，因此我们的闭包是一个有效的端点（Endpoint）。这是因为，`Into<Response>` 是为其它几种可以快捷设定端点的类型实现的。例如，下述实例中，端点（endpoint）使用由 `use tide::prelude::*` 提供的 `json!` 宏，返回 `serde_json::Value`。

```rust
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

返回字符串或 json 结果，对于获得能运作的端点（endpoint）很便捷。但是对于更多的操作，需要返回完整的响应结构体 `Response`。

```rust
server.at("*").get(|_| async {
    Ok(Response::new(StatusCode::Ok).set_body("Hello world".into()))
});
```

下一章中，将更详细地描述 `Response` 类型。

可以通过方法链添加多个端点（endpoint）。例如，如果我们想回应一个 `delete` 请求以及一个 `get` 请求，那么可以为两者各自添加端点（endpoint）；

```rust
server.at("*")
    .get(|_| async { Ok("Hello, world!") })
    .delete(|_| async { Ok("Goodbye, cruel world!") });
```

最后，当我们的端点（endpoint）方法一点点增长时，路由的定义将变得拥挤。我们可以将端点（endpoint）实现转移到它们自己的函数中；

```rust
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

## 定义和组织路由

以上过程中，我们构建的服务器仍然不是很有用。它对任何 URL 都返回相同响应，并且只能通过 HTTP 方法区分请求。我们已经使用`服务器（Server）`的 `.at` 方法定义了通配符路由，你可能已经猜到了如何向指定路由添加端点（endpoint）；

```rust,ignore
#[async_std::main]
async fn main() -> tide::Result<()> {
    let mut server = tide::new();

    server.at("/hello").get(|_| async { Ok("Hello, world!") });
    server.at("/bye").get(|_| async { Ok("Bye, world!") });

    server.listen("127.0.0.1:8080").await?;
    Ok(())
}
```

示例中，我们为两个不同的端点各自添加了路由。也可以通过链接多个 `.at` 方法来组合路由。

```rust
server.at("/hello").at("world").get(|_| async { Ok("Hello, world!") });
```

与如下写法得到的结果相同：

```rust
server.at("/hello/world").get(|_| async { Ok("Hello, world!") });
```

我们也可以存储部分路径，后续对其重用；

```rust
#[async_std::main]
async fn main() -> tide::Result<()> {
    let mut server = tide::new();

    let hello_route = server.at("/hello");

    hello_route.get(|_| async { Ok("Hi!") });
    hello_route.at("world").get(|_| async { Ok("Hello, world!") });
    hello_route.at("mum").get(|_| async { Ok("Hi, mum!") });

    server.listen("127.0.0.1:8080").await?;
    Ok(())
}
```

上面的示例中，我们向 `hello` 路由添加了两个子路由。一个是 `/hello/world`，另一个是 `hello/mum`，并且各自具有不同的端点（endpoint）函数。我们还可以为路由 `/hello` 添加端点（endpoint）。这给了我们思路，那就是建立更复杂的路由树。

当您有一个复杂的 api 时，上述用法允许您在不同的函数中定义路由树的不同部分。

```rust
#[async_std::main]
async fn main() -> tide::Result<()> {
    let mut server = tide::new();

    set_v1_routes(server.at("/api/v1"));
    set_v2_routes(server.at("/api/v2"));

    server.listen("127.0.0.1:8080").await?;
    Ok(())
}

fn v1_routes(route: Route) {
    route.at("version").get(|_| async { Ok("Version one") });
}

fn v2_routes(route: Route) {
    route.at("version").get(|_| async { Ok("Version two") });
}
```

这个示例展示了暴露两个不同版本 API 的用法，每个版本的路由在各自的函数中定义。

## 通配符

我们可以使用两个通配符字符 `:` 和 `*`。前面的端点（endpoint）示例中，我们已经用到了 `*` 通配符。两个通配符都可以匹配路由段——路由中，使用斜杠 `/` 隔开的的各部分，称之为路由段——`:` 将只匹配一个路由段，而 '*' 将匹配一个或多个路由段。

例如，`"/foo/*/baz"` 将可以匹配 `"/foo/bar/baz"` 或者 `"/foo/bar/qux/baz"`。

"foo/:/baz" 将匹配 "/foo/bar/baz"，但不匹配 "/foo/bar/qux/baz"。后者在 `foo` 和 `baz` 之间有两个路由段，而 `:` 仅匹配单个路由段。

### 命名通配符

通配符也可以用来命名。这允许您查询通配符匹配的特定字符串。例如，`"/:bar/*baz"` 将匹配字符串 `"/one/two/three"`。命名通配符可以用来查询哪些通配符匹配字符串的哪些部分。示例中，`bar` 匹配 `one`，`baz` 匹配 `two/three`。在下一章中，我们将看到如何使用命名通配符来解析 url 中的参数。

### 通配符优先权

使用通配符时，可以定义匹配同一路径的多个不同路由。

例如，路由 `"/some/*"` 和 `"/some/specific/*"` 都将匹配路径 `"/some/specific/route"`。在很多 web 框架中，路由的定义顺序将决定匹配哪个路由。Tide 将匹配模式最具体的路由。在示例中，路由 `"/some/specific/*"` 将与路径匹配。
