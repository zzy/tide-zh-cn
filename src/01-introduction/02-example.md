# 实例

> [01-introduction/02-example.md](https://github.com/http-rs/tide-book/blob/main/src/01-introduction/02-example.md)
> <br />
> commit - e24c21db3ffe7528e050eb8697b2112900157cae - 2020.10.28

下面的示例中，将创建 HTTP 服务器，接收 JSON 文本，对其进行验证，并用确认消息进行响应。

```rust,edition2018,no_run
{{#include ../../examples/ch01-02-example/src/main.rs:example}}
```

执行如下命令：

```sh
$ curl localhost:8000/orders/shoes -d '{ "name": "Chashu", "legs": 4 }'
```
输出如下响应信息：

```
Hello, Chashu! I've put in an order for 4 shoes
```

执行如下命令：

```sh
$ curl localhost:8000/orders/shoes -d '{ "name": "Mary Millipede", "legs": 750 }'
```

将没有响应信息，因为数字太大，无法放入目标类型。
