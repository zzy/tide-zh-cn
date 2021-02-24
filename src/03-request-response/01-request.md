# 请求

> [03-request-response/01-request.md](https://github.com/http-rs/tide-book/blob/main/src/03-request-response/01-request.md)
> <br />
> commit - 1a79255f4ec17387331e9fd23390ffdf9928309d - 2021.02.19

Tide 中，`Request` 结构体是端点（Endpoint）处理函数的输入参数。它包含来自 HTTP 请求的所有数据，但是 Tide 也使用 `Request` 结构体来传输应用程序和请求的`状态`。我们将在下一章更详细地讨论这个知识点。现在，你只需知道 `Request<State>` 类型中的 `State` 泛型参数是应用程序的状态即可（译者注：在大多数简单示例中，我们不会使用 `State` 泛型参数，而是使用 `Request<()>`）。

## 请求主体

`Request` 提供了一组方法来访问 `请求（Request）`主体。`body_string`、`body_bytes`，以及 `body_json` 允许您以字符串、二进制数据，或 json 数据的形式读取请求主体。

这方面需要注意几个事项：
- 首先，因为请求主体可以是一个相当大的数据块，所以这些方法是异步工作的，只需调用一次；
- 其次，`body_json` 在其返回类型上是泛型的，是故 Json 数据可以解析为实现（或派生）了 `serde::Deserialize` 的任何类型。

## 查询字符串和查询参数

等待官方文档

## 访问 Url 参数

在上一章中，我们讨论了匹配 Url 路由，特别是在关于通配符的段落中，我们已经提到了 Url 参数。例如，任何带有这样命名通配符的路由：

```rust,ignore
{{#include ../../examples/ch03-request-response/src/main.rs:url-params-route}}
```

任何用于匹配通配符的值，都可以使用 `request.param` 方法进行检索：

```rust,ignore
{{#include ../../examples/ch03-request-response/src/main.rs:url-params-handler}}
```

## HTTP 请求标头

等待官方文档
