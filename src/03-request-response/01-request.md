# 请求

> [03-request-response/01-request.md](https://github.com/http-rs/tide-book/blob/main/src/03-request-response/01-request.md)
> <br />
> commit - 1a79255f4ec17387331e9fd23390ffdf9928309d - 2021.02.19

Tide 中，`Request` 结构体是端点（Endpoint）处理函数的输入参数。它包含来自 HTTP 请求的所有数据，但是 Tide 也使用 `Request` 结构体来传输应用程序和请求的`状态`。我们将在下一章更详细地讨论这个知识点。现在，你只需知道 `Request<State>` 类型中的 `State` 泛型参数是应用程序的状态即可。在大多数简单示例中，我们不会使用 `State` 泛型参数，而是使用 `Request<()>`。

The Tide `Request` struct is the input to your endpoint handler functions. It contains all the data from the HTTP request but it is also used by Tide to pass in the application and request `State`. We will look at this in more detail in the next chapter. For now it is enough to know that the `State` generic type parameter of the `Request<State>` type you will see everywhere is the application state.


## 请求主体

The `Request` provides a set of methods to access the `Request` body. `body_string`, `body_bytes` and `body_json` allow you to read the request body either as a string, as binary data or parse it as json data.
There are a couple of things to keep in mind here. First of all is that, because a request body can be a sizable piece of data, these methods work asynchronously and can only be called once.
The `body_json` is generic over its return type. Json data can be parsed into any type that implements (or derives) `serde::Deserialize`.

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
