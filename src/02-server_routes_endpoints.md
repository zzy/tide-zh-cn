# 服务器、路由、端点（Endpoints）

> [02-server_routes_endpoints.md](https://github.com/http-rs/tide-book/blob/main/src/02-server_routes_endpoints.md)
> <br />
> commit - 32c87443efd06c62d0fab49e93e93605b1a7acb6 - 2020.12.02

tide 应用程序的中枢部分是服务器结构体 `Server`。通过创建`服务器（Server）`，并对其配置`路由（Route）`和`端点（Endpoint）`，即可启动 tide 应用程序。当`服务器（Server）`启动时，通过对传入`请求（Request）` 的 URL 和路由进行匹配，来处理传入`请求（Request）`。匹配路由的请求，随后被调度到相应的`端点（Endpoint）`。

## 设置服务器

使用 `tide::new()` 方法构建基本的 tide `服务器`。

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

虽然这是你所能构建的最简单的 tide 应用程序，但它并不是很有用。它将对任何请求返回 404 HTTP 响应。为了能够返回任何有用的信息，我们需要使用一个或多个`端点（Endpoint）`来处理请求。

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

我们使用 `at` 方法指定到达端点（Endpoint）的路由（我们稍后再讨论路由）。目前，我们仅使用 `"*"` 通配符路由，它将与我们抛出的任何内容相匹配。示例中，我们添加了一个异步闭包作为`端点（Endpoint）`。tide 期望 `at` 方法指定的`端点（Endpoint）`路由都实现 `Endpoint` trait。但实例中的闭包是有效的，因为 tide 使用如下签名实现了某些异步函数的 `Endpoint` trait；

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

The server we built is still not very useful. It will return the same response for any URL. It is only able to differentiate between requests by HTTP method. We already used the `.at` method of the `Server` to define a wildcard route. You might have guessed how to add endpoints to specific routes;

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

Here we added two routes for two different endpoints. Routes can also be composed by chaining the `.at` method.
```rust
server.at("/hello").at("world").get(|_| async { Ok("Hello, world!") });
```
This will give you the same result as:
```rust
server.at("/hello/world").get(|_| async { Ok("Hello, world!") });
```

We can store the partial routes and re-use them;
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
Here we added two sub-routes to the `hello` route. One at `/hello/world` and another one at `hello/mum` with different endpoint functions. We also added an endpoint at `/hello`. This gives an idea what it will be like to build up more complex routing trees

When you have a complex api this also allows you to define different pieces of your route tree in separate functions.
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
This example shows for example an API that exposes two different versions. The routes for each version are defined in a separate function.

## Wildcards
There are two wildcard characters we can use `:` and `*`. We already met the `*` wildcard. We used it in the first couple of endpoint examples.
Both wildcard characters will match route segments. Segments are the pieces of a route that are separated with slashes. `:` will match exactly one segment while '*' will match one or more segments.

`"/foo/*/baz"` for example will match against `"/foo/bar/baz"` or `"/foo/bar/qux/baz"`

"foo/:/baz" will match "/foo/bar/baz" but not "/foo/bar/qux/baz", the latter has two segments between foo and baz, while `:` only matches single segments.

### Naming wildcards
It is also possible to name wildcards. This allows you to query the specific strings the wildcard matched on. For example `"/:bar/*baz"` 
will match the string `"/one/two/three"`. You can then query which wildcards matched which parts of the string. In this case `bar` matched `one` while `baz` matched `two/three`. We'll see how you can use this to parse parameters from urls in the next chapter.

### Wildcard precedence
When using wildcards it is possible to define multiple different routes that match the same path.

The routes `"/some/*"` and `"/some/specific/*"` will both match the path `"/some/specific/route"` for example. In many web-frameworks the order in which the routes are defined will determine which route will match. Tide will match the most specific route that matches. In the example it the `"/some/specific/*"` route will match the path.
