# 入门

> [01-introduction/01-getting_started.md](https://github.com/http-rs/tide-book/blob/main/src/01-introduction/01-getting_started.md)
> <br />
> commit - ac6f8a5cd1619f9a519c40df428a2c20bd992183 - 2020.12.02

为了在 Rust 中构建 web 应用程序，你需要一个 HTTP 服务器，以及一个异步运行时。在运行 `cargo new --bin web-app` 命令后，将如下行添加到 `Cargo.toml` 文件中：

```toml
# 仅为示例，请使用你需要的版本
tide = "0.15.0"
async-std = { version = "1.6.5", features = ["attributes"] }
```
