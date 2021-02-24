# 介绍

> [introduction/00-introduction.md](https://github.com/http-rs/tide-book/blob/main/src/introduction/00-introduction.md)
> <br />
> commit - 9a6be55d3b57c6334239f37505b968ece5eda2c4 - 2021.02.18

Tide 是小型而实用的 Rust web 应用程序框架，为快速开发而构建。它提供了一组健壮的特性，使得构建异步 web 应用程序和 API 更加容易、更为有趣。

Tide 指导手册正在编写中，会随着实践的推移逐步完善。

本书中所有的实例请参阅 [Tide 实例项目](https://github.com/http-rs/tide-book/tree/main/examples)。

> 💥 Tide 在**生产环境的实践示例**项目，请参阅 [yazhijia（github 仓库）](https://github.com/zzy/yazhijia)（将持续升级）：
> - 纯粹 Rust 技术栈实现的博客系统，有兴趣请访问[演示站点](https://blog.budshome.com)：。
> - 后端（backend）主要提供 graphql 服务，使用了 tide, async-graphql, jsonwebtoken, mongodb 等相关 crate。
> - 前端（frontend）提供 web 应用服务，使用了 tide, rhai, surf, graphql_client, handlebars-rust, cookie 等相关 crate。
> 
> 💥 关于清洁的模板项目，采用了**纯粹的 Rust 技术栈**。包括（将持续升级）：
> - [Rust](https://www.rust-lang.org) - [中文资料集萃](https://budshome.com)
> - [Tide](https://crates.io/crates/tide) - [中文文档](https://tide.budshome.com)
> - [async-graphql](https://crates.io/crates/async-graphql) - [中文文档](https://async-graphql.budshome.com)
> - [mongodb & mongo-rust-driver](https://crates.io/crates/mongodb)
> - [Surf](https://crates.io/crates/surf)
> - [graphql_client](https://crates.io/crates/graphql_client)
> - [handlebars-rust](https://crates.io/crates/handlebars)
> - [jsonwebtoken](https://crates.io/crates/jsonwebtoken)
> - [cookie-rs](https://crates.io/crates/cookie)
>
> 请参阅 github 仓库 <a href="https://github.com/zzy/tide-graphql-mongodb" target="_blank">**tide-graphql-mongodb**</a>。目前实现了如下功能（将持续升级）：
> - 用户注册
> - 使用 PBKDF2 对密码进行加密（salt）和散列（hash）运算
> - 整合 JWT 鉴权的用户登录
> - 密码修改
> - 资料更新
> - 用户查询和变更
> - 项目查询和变更
> - 使用基于 Rust 实现 graphql-client 获取 GraphQL 服务端数据
> - 渲染 GraphQL 数据到 handlebars-rust 模板引擎
