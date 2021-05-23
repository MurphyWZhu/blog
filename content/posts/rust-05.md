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

```rust
let v = vec![1, 2, 3, 4, 5];

let third: &i32 = &v[2];
println!("The third element is {}", third);

match v.get(2) {
    Some(third) => println!("The third element is {}", third),
    None => println!("There is no third element."),
}
```

下面代码会报错

```rust
let mut v = vec![1, 2, 3, 4, 5];
let first = &v[0];
v.push(6);//加入一个元素会使v重新分配一块连续的地址，所以原来的引用就无效了，rust不允许无效引用
println!("The first element is: {}", first);
```

遍历`vector`

```rust
let v = vec![1, 2, 3, 4, 5];
for i in &v {
    println!("{}", i);
}
```

使用枚举来存储多种类型

`vector`只能存储一种类型，而枚举算一种类型，可以用枚举来存储多种类型

```rust

#![allow(unused)]
fn main() {
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
}
```

### 字符串

