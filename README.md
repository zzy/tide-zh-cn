# Tide 中文文档

[Build Status travis]: https://api.travis-ci.com/zzy/tide-zh-cn.svg?branch=master
[travis]: https://travis-ci.com/zzy/tide-zh-cn

Tide 是 Rust 生态中的最为优秀的 web 框架之一，具有类型安全、功能丰富、扩展性强，以及速度极快的诸多优点。

本仓库为《Tide 中文文档》，同步、整理、实践，以及拓展自 Tide 团队仓库和官网。

**感谢 Tide 团队的无私奉献！**

## 在线阅读

在线阅读地址：[**《Tide 中文文档》** - https://tide-book.niqin.com](https://tide-book.niqin.com)。

## 离线阅读

如果你喜欢本地阅读方式，可以使用 mdBook（[中文文档](https://mdbook.niqin.com)） 进行书籍构建：

> 构建时需要安装一些 crate，中国大陆推荐[更换默认的 Cargo 源为国内镜像源](https://cargo.niqin.com/reference/source-replacement.html)。

```bash
$ git clone https://github.com/zzy/tide-zh-cn
$ cd tide-zh-cn
$ cargo install mdbook # 指定版本使用参数：--vers "0.3.5"
$ mdbook serve --open # 或者 mdbook build
```

也可以直接用你喜欢的浏览器从 `book` 子目录打开 `index.html` 文件。

```bash
$ xdg-open ./book/index.html # linux
$ start .\book\index.html    # windows
$ open ./book/index.html     # mac
```

## 贡献

《Tide 中文文档》的目的是让 Rust 程序员新手能够更容易地参与到 Tide 社区中，因此它需要——并欢迎——你做出自己力所能及的贡献。

祝你学习愉快，欢迎提交问题，欢迎发送 PR。
