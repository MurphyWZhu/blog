---
title: "Rust智能指针"
date: 2021-06-05T18:51:39+08:00
draft: false
tags: ["Rust"]
---

### 使用`Box<T>`指向堆上的数据

使用`box`在堆上储存一个`i32`

`b`是一个指向被分配在堆上的值`5`的`Box`

```rust
fn main() {
    let b = Box::new(5);
    println!("b = {}", b);
}
```

