# 服务器和请求状态

> [04-state.md](https://github.com/http-rs/tide-book/blob/main/src/04-state.md)
> <br />
> commit - 61cacd9f9ec4c8953a56839de286f551087b4c39 - 2021.01.22

## 服务器状态

到目前为止，示例中的端点（Endpoint）都是简单的无状态函数，将请求处理为响应。但是对于任何正式的应用程序，我们需要能够在某个地方保持某种状态。在实际的应用程序中，我们需要有一个地方来存储会话（session）、数据库连接池、配置等内容，而我们不希望使用全局变量来实现这一点。

Tide 为我们提供了服务器状态来执行上述操作。如果你查看服务器定义的 `Server` 结构体，就会发现它有一个名称为 `State` 的泛型参数。因为到目前为止，我们一直使用 `Server::new` 工厂方法创建服务器结构体 `Server`，所以我们一直使用的是 `Server<()>`。但是我们可以在创建服务器时，传入自定义的状态结构体或状态枚举。然后，状态结构体或状态枚举将通过请求结构体 `Request` 传递到所有端点（Endpoint）处理程序。

### 为应用程序设定状态

要为我们的应用程序设定状态，首先需要有一个类型，此类型用来存储将在请求之间共享的应用程序数据。

```rust,ignore
{{#include ../examples/ch04-server-state/src/main.rs:appstate}}
```

在本例中，我们将共享一个简单的计数器。我们使用 Arc<AtomicU32> 来确保可以安全地共享这些信息，即使是在同时出现请求的情况下。

欲在 `tide::server` 中设置状态，我们需要使用与之前 `server::new()` 不同的构造函数。我们可以使用 `server::with_state(...)` 来设置具有状态（state）的服务器。

```rust,ignore
{{#include ../examples/ch04-server-state/src/main.rs:start_with_state}}
```

### 访问应用程序状态

```rust,ignore
{{#include ../examples/ch04-server-state/src/main.rs:read_state_request}}
```

```rust,ignore
{{#include ../examples/ch04-server-state/src/main.rs:update_state_request}}
```

### 受限的状态

正如您在前面的示例中所看到的，`状态（State）`对象确实有一些限制。首先，Tide 设定了一些 trait 界限。`状态（State）`需要可 `Clone`、`Send`，以及 `Sync`。这是因为：状态将被传递到所有应用程序端点（endpoint）。它们可能同时运行，并且是在不同的线程上运行，而不是在创建`状态（State）`的线程上运行。

`状态（State）`也会被返回为不可变（non-mutable）引用。

为了绕过计数器的这些限制，我们使用了 AtomicU32，它为计数器提供了内部可变性。并且，我们将其包装成 `Arc`，以便能够复制它。

实际上，通常来说这并非什么大问题。例如，许多数据库访问类库已经提供了连接池组件，这些组件被编写为能够在应用程序内部传递。<TODO>待完成：指向 sqlx 示例</TODO>。
