# 请求和响应

> [03-request-response/00-request-response.md](https://github.com/http-rs/tide-book/blob/main/src/03-request-response/00-request-response.md)
> <br />
> commit - 276628d558aaeeb45b85cb33f80e939e062b6104 - 2021.01.21

在上一章中，我们了解到端点（Endpoint）是接收`请求（Request）`和返回`响应（Response）`的函数。或者更准确地说，端点（Endpoint）是一个具有可转换为`响应（Response）`类型的枚举 `Result`。

```rust,ignore
async fn endpoint(request: tide::Request) -> tide::Result<impl Into<Response>>
```

`Request` 对象包含服务器接收到的来自 HTTP 请求的所有信息。如请求中的 URL、HTTP 消息标头、cookies，以及查询字符串参数等，这些都可以在 `Request` 结构体中找到。此外，Tide 中的 `Request` 对象用于将应用程序状态和请求状态的相关信息传递到端点（Endpoint）。下一章是`状态（State）`部分，我们将对此进行研究。

`Response` 结构体允许我们构建完整的 HTTP 响应。它包含 HTTP 响应的主体，也包含一组 HTTP 消息标头和响应码。虽然可以直接创建、访问和修改 `Response` 结构体，但是通过 Tide 内置的构建器 `ResponseBuilder` 创建 `Response` 非常方便。
