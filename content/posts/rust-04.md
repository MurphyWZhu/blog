---
title: "Rust枚举"
date: 2021-05-22T20:03:35+08:00
draft: false
tags: ["Rust"]
---

### 定义枚举

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

### 枚举值

```rust
let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
```

他们的类型都是`IpAddrKind`

如函数：

```rust
fn rout(ip_type: IpAddrKind) { }
```

可以向下面一样调用

```rust
route(IpAddrKind::V4);
route(IpAddrKind::V6);
```

