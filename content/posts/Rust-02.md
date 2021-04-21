---
title: "Rust所有权"
date: 2021-04-21T21:45:28+08:00
draft: false
tags: ["Rust"]
---

### 所有权

所有权是`rust`的核心之一

它有如下规则

```
1、Rust 中的每一个值都有一个被称为其 所有者（owner）的变量。
2、值在任一时刻有且只有一个所有者。
3、当所有者（变量）离开作用域，这个值将被丢弃。
```

### 变量作用域

```rust
#![allow(unused)]
fn main() {
{                      // s 在这里无效, 它尚未声明
    let s = "hello";   // 从此处起，s 是有效的

    // 使用 s
}                      // 此作用域已结束，s 不再有效
}
```

#### `String`类型

之前的类型都是栈上的，现在需要一个堆上的类型，`String`就是我们需要的类型

```rust
let s = String::from("hello");
```

对于栈上的数据`rust`直接复制了

```rust
let x = 5;
let y = x;
```

对于堆上的数据，`rust`将原来的内存给了之后的，而原来的就无效了

如下代码会错误，因为`s1`无效了，所以发生了无效引用

```rust
let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1);
```

如果要复制`String`的数据可以使用`clone`

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2);
```

### 所有权与函数

```rust
fn main() {
    let s = String::from("hello");  // s 进入作用域

    takes_ownership(s);             // s 的值移动到函数里 ...
                                    // ... 所以到这里不再有效

    let x = 5;                      // x 进入作用域

    makes_copy(x);                  // x 应该移动函数里，
                                    // 但 i32 是 Copy 的，所以在后面可继续使用 x

} // 这里, x 先移出了作用域，然后是 s。但因为 s 的值已被移走，
  // 所以不会有特殊操作

fn takes_ownership(some_string: String) { // some_string 进入作用域
    println!("{}", some_string);
} // 这里，some_string 移出作用域并调用 `drop` 方法。占用的内存被释放

fn makes_copy(some_integer: i32) { // some_integer 进入作用域
    println!("{}", some_integer);
} // 这里，some_integer 移出作用域。不会有特殊操作
```



```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership 将返回值
                                        // 移给 s1

    let s2 = String::from("hello");     // s2 进入作用域

    let s3 = takes_and_gives_back(s2);  // s2 被移动到
                                        // takes_and_gives_back 中,
                                        // 它也将返回值移给 s3
} // 这里, s3 移出作用域并被丢弃。s2 也移出作用域，但已被移走，
  // 所以什么也不会发生。s1 移出作用域并被丢弃

fn gives_ownership() -> String {             // gives_ownership 将返回值移动给
                                             // 调用它的函数

    let some_string = String::from("hello"); // some_string 进入作用域.

    some_string                              // 返回 some_string 并移出给调用的函数
}

// takes_and_gives_back 将传入字符串并返回该值
fn takes_and_gives_back(a_string: String) -> String { // a_string 进入作用域

    a_string  // 返回 a_string 并移出给调用的函数
}
```



```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() 返回字符串的长度

    (s, length)
}
```

