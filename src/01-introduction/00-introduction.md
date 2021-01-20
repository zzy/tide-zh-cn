# 介绍

> [01-introduction/00-introduction.md](https://github.com/http-rs/tide-book/blob/main/src/01-introduction/00-introduction.md)
> <br />
> commit - 69aaddaaf2e73792b8050ccf7931c2e8ed5378da - 2020.10.27

Tide 是小型而实用的 Rust web 应用程序框架，为快速开发而构建。它提供了一组健壮的特性，使得构建异步 web 应用程序和 API 更加容易、更为有趣。

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
> 请参阅 github 仓库 <a href="https://github.com/zzy/tide-handlebars-graphql-mongodb" target="_blank">**tide-handlebars-graphql-mongodb**</a>。目前实现了如下功能（将持续升级）：
> - 用户注册
> - 使用 PBKDF2 对密码进行加密（salt）和散列（hash）运算
> - 整合 JWT 鉴权的用户登录
> - 密码修改
> - 资料更新
> - 用户查询和变更
> - 项目查询和变更
> - 使用基于 Rust 实现 graphql-client 获取 GraphQL 服务端数据
> - 渲染 GraphQL 数据到 handlebars-rust 模板引擎
