# 请求和响应

> [03-request-response.md](https://github.com/http-rs/tide-book/blob/main/src/03-request-response.md)
> <br />
> commit - 8765e71b37816e22d87517112ff33495f233a684 - 2020.12.03

在上一章中，我们了解了端点（endpoint）是接收`请求（Request）`和返回`函数（Response）`的函数，或者更准确地说，端点（endpoint）是一个具有可转换为`响应（Response）`类型的枚举 `Result`。

```rust
async fn endpoint(request: tide::Request) -> tide::Result<impl Into<Response>>
```

`请求（Request）`对象包含服务器接收到的来自HTTP请求的所有信息。请求中的URL、HTTP头、cookies和查询字符串参数都可以在请求中找到。此外，Tide中的Request对象用于将有关应用程序状态和请求状态的信息传递到端点。我们将在下一章关于国家的章节中对此进行研究。


反过来，响应结构允许我们构建一个完整的HTTP响应。它包含响应主体，但也包含一组HTTP头和一个响应代码。虽然可以直接创建、访问和修改响应结构，但是通过Tide ResponseBuilder创建响应非常方便。

The `Request` object contains all the information from the HTTP request that was received by the server. The URL from the request, HTTP headers, cookies and query string parameters can all be found in the `Request`.
Additionally the `Request` object in Tide is used to pass information about the application state and the request state into the endpoint. We will look into this in the next chapter about `State`.

The `Response` struct in turn allows us to craft a complete HTTP response. It contains the `Response` body, but also a set of HTTP headers and a response code. While the `Response` struct can be created, accessed and modified directly, it can be convenient to create a `Response` through the Tide `ResponseBuilder`.

## Request
The Tide `Request` struct is te input to your endpoint handler function. It contains all the data from the HTTP request but it is also used by Tide to pass in the application and request `State`. We will look at this in more detail in the next chapter. For now it is enough to know that the `State` generic type parameter of the `Request<State>` type you will see everywhere is the application state. In most simple examples we will not use this state and you will see `Request<()>`.

### Request body

### Accessing Url parameters
In the last chapter where we talked about matching Url routes and specifically in the paragraph about wildcards we already mentioned Url-parameters.
From any route with named wildcards like this;
```rust
{{#include ../examples/ch03-request-response/src/main.rs:url-params-route}}
```
Any value that was used to match the a wildcard can be retrieved using the `request.param` method;
```rust
{{#include ../examples/ch03-request-response/src/main.rs:url-params-handler}}
```

### The query string and query parameters

### HTTP request headers

## Response and the ResponseBuilder

