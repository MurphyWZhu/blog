---
title: "Rust集合"
date: 2021-05-23T11:48:49+08:00
draft: false
---

### `vector`

`vector`在内存中是连续的，只能存储相同类型的值

新建`vector`

```rust
let v: Vec<i32> = Vec::new();
```

或者使用`rust`提供的`vec!`宏来新建一个`vector`

```rust
let v = vec![1, 2, 3];
```

更新`vector`

```rust
let mut v = Vec::new();//需要可变才能改变
v.push(5);
```

读取`vector`的元素

```
```

