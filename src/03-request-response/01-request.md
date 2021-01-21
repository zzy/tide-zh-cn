## 请求

> [03-request-response/01-request.md](https://github.com/http-rs/tide-book/blob/main/src/03-request-response/01-request.md)
> <br />
> commit - 276628d558aaeeb45b85cb33f80e939e062b6104 - 2021.01.21

Tide 中，`Request` 结构体是端点（Endpoint）处理函数的输入参数。它包含来自 HTTP 请求的所有数据，但是 Tide 也使用 `Request` 结构体来传输应用程序和请求的`状态`。我们将在下一章更详细地讨论这个知识点。现在，你只需知道 `Request<State>` 类型中的 `State` 泛型参数是应用程序的状态即可。在大多数简单示例中，我们不会使用 `State` 泛型参数，而是使用 `Request<()>`。

### 请求主体

### 访问 Url 参数

在上一章中，我们讨论了匹配 Url 路由，特别是在关于通配符的段落中，我们已经提到了 Url 参数。例如，任何带有这样命名通配符的路由：

```rust,ignore
{{#include ../../examples/ch03-request-response/src/main.rs:url-params-route}}
```

任何用于匹配通配符的值，都可以使用 `request.param` 方法进行检索：

```rust,ignore
{{#include ../../examples/ch03-request-response/src/main.rs:url-params-handler}}
```

### 查询字符串和查询参数

等待官方文档

### HTTP 请求标头

等待官方文档

## 响应和响应构建器（ResponseBuilder）

等待官方文档
