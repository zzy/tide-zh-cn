# 请求和响应

> [03-request-response.md](https://github.com/http-rs/tide-book/blob/main/src/03-request-response.md)
> <br />
> commit - 8765e71b37816e22d87517112ff33495f233a684 - 2020.12.03

在上一章中，我们了解了端点（endpoint）是接收`请求（Request）`和返回`函数（Response）`的函数，或者更准确地说，端点（endpoint）是一个具有可转换为`响应（Response）`类型的枚举 `Result`。

```rust
async fn endpoint(request: tide::Request) -> tide::Result<impl Into<Response>>
```

`Request` 对象包含服务器接收到的来自 HTTP 请求的所有信息。请求中的URL、HTTP 消息标头、cookies，以及查询字符串参数，这些都可以在 `Request` 结构体中找到。此外，tide 中的 `Request` 对象用于将应用程序状态和请求状态的相关信息传递到端点（endpoint）。下一章是`状态（State）`部分，我们将对此进行研究。

`Response` 结构体允许我们构建完整的 HTTP 响应。它包含 HTTP 响应的主体，也包含一组 HTTP 消息标头和响应码。虽然可以直接创建、访问和修改 `Response` 结构体，但是通过 Tide 内置的 `ResponseBuilder` 构建器创建 `Response` 非常方便。

## 请求

Tide 中，`Request` 结构体是端点（endpoint）处理函数的输入。它包含来自 HTTP 请求的所有数据，但是 Tide 也使用 `Request` 结构体来传输应用程序和请求的`状态`。我们将在下一章更详细地讨论这个问题。现在，你只需知道 `Request<State>` 类型中的 `State` 泛型参数是应用程序的状态即可。在大多数简单示例中，我们不会使用 `State` 泛型参数，而是使用 `Request<()>`。

### 请求主体

### 访问 Url 参数

在上一章中，我们讨论了匹配 Url 路由，特别是在关于通配符的段落中，我们已经提到了 Url 参数。例如，任何带有这样命名通配符的路由：

```rust
{{#include ../examples/ch03-request-response/src/main.rs:url-params-route}}
```

任何用于匹配通配符的值，都可以使用 `request.param` 方法进行检索：

```rust
{{#include ../examples/ch03-request-response/src/main.rs:url-params-handler}}
```

### 查询字符串和查询参数

等待官方文档

### HTTP 请求标头

等待官方文档

## 响应和响应构建器（ResponseBuilder）

等待官方文档
