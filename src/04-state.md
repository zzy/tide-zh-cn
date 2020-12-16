# 服务器和请求状态

> [04-state.md](https://github.com/http-rs/tide-book/blob/main/src/04-state.md)
> <br />
> commit - 3f00bc7f7b7dd93a7f684add8a4ab856f27dbf56 - 2020.10.24

## 服务器状态

到目前为止，端点都是简单的无状态函数，将请求处理为响应。但是对于任何严肃的应用程序，我们需要能够在某个地方保持某种状态。在实际应用程序中，我们需要有一个地方来存储会话、数据库连接池、配置等内容，而我们不希望使用全局变量来实现这一点。


Tide为我们提供了服务器状态来执行此操作。如果您查看服务器结构的定义，就会发现它有一个名为State的泛型类型参数。因为到目前为止，我们一直使用Server:：new factory方法创建服务器，所以我们一直使用Server<（）>。但是我们可以在创建服务器时传入自己的状态结构或枚举。然后，它将通过请求传递到所有端点处理程序。

Until now endpoints were simple stateless functions that processed a request into a response. But for any serious application we would need to be able to maintain some state somewhere. In a real life application we would need to have a place to store things like sessions, database connection pools, configuration etc. And we would rather not use global variables to do this.

Tide gives us Server state to do just this. If you look at the definition of the Server struct you see that it has one generic type parameter called `State`. Because we've been creating `Server`s with the `Server::new` factory method so far we have been using `Server<()>`. But we can pass in our own state struct or enum when creating the Server. This will then be passed into all endpoint handlers through the `Request`.
