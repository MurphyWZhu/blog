---
title: "Rust常见编程概念"
date: 2021-04-21T14:55:05+08:00
draft: false
tags: ["Rust"]
# cover:
    # image: "https://mublog.oss-cn-beijing.aliyuncs.com/ec16e275a07709bf0043bda9608de846.jpeg"
    # alt: "Rust"
    # caption: "ArchLinux"
    # relative: false # To use relative path for cover image, used in hugo Page-bundle
---

### 不变变量

以下程序理所当然的输出`5`

```rust
fn main() {
    let a = 5;
    println!("{}", a);
}
```

而当你尝试改变`a`的值时

```rust
fn main() {
    let a = 5;
    a = 6;
    println!("{}", a);
}
```

则会出现错误

```rust
error[E0384]: cannot assign twice to immutable variable `a`
 --> src/main.rs:3:5
  |
2 |     let a = 5;
  |         -
  |         |
  |         first assignment to `a`
  |         help: make this binding mutable: `mut a`
3 |     a = 6;
  |     ^^^^^ cannot assign twice to immutable variable
```

`rust`会很人性化的提示你哪里错了，还会给出建议

这就是因为`rust`变量默认是不可改变的，要改变的话需要声明他是可变的，就像他提示的那样

```rust
fn main() {
    let mut a = 5;
    a = 6;
    println!("{}", a);
}
```

### 常量

`rust`变量默认不可变，那好像和常量一样，常量不光默认不能变，它总是不能变。常量只能被设置为常量表达式，而不能是函数调用的结果，或任何其他只能在运行时计算出的值。

```rust
const MAX_POINTS: u32 = 100_000;
```

### 数据类型

声明

```rust
let x: i64 = 10;
let x = 5.5;
```

`char`类型

`rust`的`char`为四个字节，代表一个`unicode`标量值，所以可以使用`emoji`、中文字符等等

```rust
let c = 'z';
let z = 'ℤ';
let heart_eyed_cat = '😻';
```

元组`(tup)`

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
let tup = (500, 6.4, 1);
println!("{}", tup.0);
```

数组`(array)`

数组是固定长度的，一旦声明，它们的长度不能增长或缩小。

```rust
let a = [1, 2, 3, 4, 5];
let b: [i32; 5] = [1, 2, 3, 4, 5];
let c = [3; 5];//[3,3,3,3,3]
println!("{}", a[0]);
```

### 函数

```rust
fn func_name(x: i32) -> i32 {
	x*2
}
```





