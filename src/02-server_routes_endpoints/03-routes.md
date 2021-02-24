# 定义和组织路由

> [02-server_routes_endpoints/03-routes.md](https://github.com/http-rs/tide-book/blob/main/src/02-server_routes_endpoints/03-routes.md)
> <br />
> commit - 759c39d68f63e06b83bedd812f2a4a3a770879f9 - 2021.02.17

以上过程中，我们构建的服务器仍然不是很有用。它对任何 URL 都返回相同响应，并且只能通过 HTTP 方法区分请求。我们已经使用`服务器（Server）`的 `.at` 方法定义了通配符路由，你可能已经猜到了如何向指定路由添加端点（Endpoint）；

```rust,edition2018,no_run
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

```rust,ignore
server.at("/hello").at("world").get(|_| async { Ok("Hello, world!") });
```

与如下写法得到的结果相同：

```rust,ignore
server.at("/hello/world").get(|_| async { Ok("Hello, world!") });
```

我们也可以存储部分路径，后续对其重用；

```rust,edition2018,no_run
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

上面的示例中，我们向 `hello` 路由添加了两个子路由。一个是 `/hello/world`，另一个是 `hello/mum`，并且各自具有不同的端点（Endpoint）函数。我们还可以为路由 `/hello` 添加端点（Endpoint）。这给了我们思路，那就是建立更复杂的路由树。

当你有一个复杂的 api 时，上述用法允许你在不同的函数中定义路由树的不同部分。

```rust,edition2018,no_run
#[async_std::main]
async fn main() -> tide::Result<()> {
    let mut server = tide::new();

    set_v1_routes(server.at("/api/v1"));
    set_v2_routes(server.at("/api/v2"));

    server.listen("127.0.0.1:8080").await?;
    Ok(())
}

fn set_v1_routes(route: Route) {
    route.at("version").get(|_| async { Ok("Version one") });
}

fn set_v2_routes(route: Route) {
    route.at("version").get(|_| async { Ok("Version two") });
}
```

这个示例展示了暴露两个不同版本 API 的用法，每个版本的路由在各自的函数中定义。

## 通配符

我们可以使用两个通配符字符 `:` 和 `*`。前面的端点（Endpoint）示例中，我们已经用到了 `*` 通配符。两个通配符都可以匹配路由段——路由中，使用斜杠 `/` 隔开的的各部分，称之为路由段——`:` 将只匹配一个路由段，而 `*` 将匹配一个或多个路由段。

例如，`"/foo/*/baz"` 将可以匹配 `"/foo/bar/baz"` 或者 `"/foo/bar/qux/baz"`。

"foo/:/baz" 将匹配 "/foo/bar/baz"，但不匹配 "/foo/bar/qux/baz"。后者在 `foo` 和 `baz` 之间有两个路由段，而 `:` 仅匹配单个路由段。

### 命名通配符

通配符也可以用来命名。这允许你查询通配符匹配的特定字符串。例如，`"/:bar/*baz"` 将匹配字符串 `"/one/two/three"`。命名通配符可以用来查询哪些通配符匹配字符串的哪些部分。示例中，`bar` 匹配 `one`，`baz` 匹配 `two/three`。在下一章中，我们将看到如何使用命名通配符来解析 url 中的参数。

### 通配符优先权

使用通配符时，可以定义匹配同一路径的多个不同路由。

例如，路由 `"/some/*"` 和 `"/some/specific/*"` 都将匹配路径 `"/some/specific/route"`。在很多 web 框架中，路由的定义顺序将决定匹配哪个路由。而在 Tide 框架中，将匹配模式最具体的路由。在示例中，路由 `"/some/specific/*"` 将与路径匹配。
