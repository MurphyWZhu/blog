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

以下程序会报错

```rust
fn main() {
    let a = String::from("Hello");
    print_str(a);
    println!("{}", a);
}

fn print_str(str : String) {
    println!("{}", str);
}
```

说`a`已经被移动了

```rust
error[E0382]: borrow of moved value: `a`
 --> src/main.rs:4:20
  |
2 |     let a = String::from("Hello");
  |         - move occurs because `a` has type `String`, which does not implement the `Copy` trait
3 |     print_str(a);
  |               - value moved here
4 |     println!("{}", a);
  |                    ^ value borrowed here after move

```

这是因为啊的所有权被移动到了`print_str`函数里，函数结束所有权就消失了

那应该怎么办呢，不用担心，函数返回值的时候也会返回所有权，我们只需要将原来的参数返回就行

```rust
fn main() {
    let a = String::from("Hello");
    let b = print_str(a);
    println!("{}", b);
}

fn print_str(str : String) -> String{
    println!("{}", str);
    str
}
```

也可以像下面一样，将要返回的东西和原来的参数一起返回，不过这样着实是有些麻烦，不过我们接下来的东西可以解决

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

### 借用

