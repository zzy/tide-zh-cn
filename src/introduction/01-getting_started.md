# 入门

> [introduction/01-getting_started.md](https://github.com/http-rs/tide-book/blob/main/src/introduction/01-getting_started.md)
> <br />
> commit - 9a6be55d3b57c6334239f37505b968ece5eda2c4 - 2021.02.18

为了在 Rust 中构建 web 应用程序，你需要 HTTP 服务器，以及异步运行时。在运行 `cargo new --bin web-app` 命令后，将如下行添加到 `Cargo.toml` 文件中：

```toml
# 仅为示例，请使用你需要的版本
tide = "0.15.0"
async-std = { version = "1.6.5", features = ["attributes"] }
```

# 示例

下面的示例中，将创建 HTTP 服务器，接收 JSON 文本，对其进行验证，并用确认消息进行响应。

```rust,edition2018,no_run
{{#include ../../examples/ch01-02-example/src/main.rs:example}}
```

执行如下命令：

```sh
$ curl localhost:8080/orders/shoes -d '{ "name": "Chashu", "legs": 4 }'
```
输出如下响应信息：

```
Hello, Chashu! I've put in an order for 4 shoes
```

执行如下命令：

```sh
$ curl localhost:8080/orders/shoes -d '{ "name": "Mary Millipede", "legs": 750 }'
```

将没有响应信息，因为数字太大，无法放入目标类型。
