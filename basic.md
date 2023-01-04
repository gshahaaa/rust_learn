# rust basic
rust为一种预编译静态类型语言，表示可以编译程序并将执行程序送给别人，而不需要安装rust;默认情况下，rust自动导入若干个标准库内容，被称为preclude。主要包括：
> std::marker::{Copy, Send, Sized, Sync, Unpin}，指示类型基本属性的标记特征。
> 
> std::ops::{Drop, Fn, FnMut, FnOnce}，析构函数和重载的各种操作()。
> 
> std::mem::drop，一个用于显式删除值的便捷函数。
> 
> std::boxed::Box，一种在堆上分配值的方法。
> 
> std::borrow::ToOwned，定义的转换特征， to_owned从借用类型创建自有类型的通用方法。
> 
> std::clone::Clone，定义 的无处不在的特征， clone用于生成值副本的方法。
> 
> std::cmp::{PartialEq, PartialOrd, Eq, Ord}，比较特征，它实现了比较运算符并且经常出现在特征边界中。
> 
> std::convert::{AsRef, AsMut, Into, From}，通用转换，由精明的 API 作者用来创建重载方法。
> 
> std::default::Default, 具有默认值的类型。
> 
> std::iter::{Iterator, Extend, IntoIterator, DoubleEndedIterator, ExactSizeIterator}, 各种迭代器。
> 
> std::option::Option::{self, Some, None}，一种表示值存在或不存在的类型。这种类型非常常用，它的变体也被出口。
> 
> std::result::Result::{self, Ok, Err}，一种可能成功或失败的函数类型。像 一样Option，它的变体也被导出。
> 
> std::string::{String, ToString}，堆分配的字符串。
> 
> std::vec::Vec，一个可增长的堆分配向量。
Rust 2021 中使用的序曲std::prelude::rust_2021，包括上述所有内容，此外还重新导出：
> 
> std::convert::{TryFrom, TryInto},
> 
> std::iter::FromIterator.
>
若要手动导入，则需要使用use关键字将其引入作用域。
## 变量
rust存在一个静态强类型系统，并且存在类型推断。并且其允许使用一个新值隐藏之前的值，常用于需要转换值类型之类的场景，隐藏时被隐藏对象并未消失，在第二个对象作用域结束之后，被隐藏的对象可以继续被使用。
```rust
 let mut guess= String::new();
 let guess:i32 = guess.trim().parse().expect("please type a number");
```
rust中使用let语句创建变量，默认创建的变量是不可变的,规定不能对不可变变量第二次赋值。这意味着，一旦被赋值，值就不可以被修改了。设计不可变变量的目的在于可以安全且方便的写出复杂，甚至是并行的代码。

如果需要修改的话，需要使用mut对变量进行修饰。mut避免对于复杂的数据类型，修改时复制病分配新的内存的损耗。
mut有两种用途：
* 在任何地方修饰可变变量
* 修饰引用

### 常量
使用const修饰，不可以对常量使用mut；并且声明常量时必须使用const以及必须注明值的类型，其可以在任何作用域包括全局作用域中声明。
```rust
const THREE:u32 = 0
```
在声明它的作用域之中，常量在整个程序生命周期中都有效，此属性使得常量可以作为多处代码使用的全局范围的值。
### 静态变量
静态变量在程序的整个声明周期都可用，且不能被shallow，被分配在编译时已知的内存块。静态项拥有 'static 生存期，它比 Rust 程序中的所有其他生存期都要长。静态项不会在程序结束时调用析构动作 drop。

在rust中，可变的静态变量在被读和写的场合，由于线程不安全特性，都需要标识为unsafe，eg.
```rust
static mut Level:i32 = 1
fn main(){
    unsafe {
        println!("{Level}");
    }
}
```

### 枚举
```rust
pub enum Result<T,E>{
    Ok(T),
    Err(E),
}
```
### 标量
| 长度 | 有符号 | 无符号 |
| --- | --- | --- |
| 8-bit | i8 | u8 |
|16-bit|i16|u16|
|32-bit|i32|u32|
|64-bit|i64|u64|
|128-bit|i128|u128|
|arch|isize|usize|

在Debug编译时出现溢出则会panic。若在release期间，会进行二进制补码回绕的操作，也可以显式处理溢出：
* 所有模式下都可以使用 wrapping_* 方法进行回绕，如 wrapping_add
* 如果 checked_* 方法出现溢出，则返回 None值
* 用 overflowing_* 方法返回值和一个布尔值，表示是否出现溢出
* 用 saturating_* 方法在值的最小值或最大值处进行饱和处理

### char
Rust 的 char 类型的大小为四个字节(four bytes)，并代表了一个 Unicode 标量值（Unicode Scalar Value），这意味着它可以比 ASCII 表示更多内容。
### 复合类型
Rust 有两个原生的复合类型：元组（tuple）和数组（array）。
**元组** 
元组是一个将多个其他类型的值组合进一个复合类型的主要方式。元组长度固定：一旦声明，其长度不会增大或缩小。
```rust
let up:(f32,i32,u8) = (2.0,2,1)
//读取方式1 解构
let (x,y,z) = up;
//读取方式2 点号.后跟值的索引
let first = up.0
```
不带任何值的元组叫做单元元组，值和类型都写作(),表示空值或者空的返回类型。如果表达式不返回任何值，则会隐式返回单元元组。
```rust
let up:() = ()
```
**数组** 
rust中的数组长度是固定的，类似与C++，存在可变长度的集合类型的vector。
```rust
//创建方式1
let a = [1,2,3,4,5]
//创建方式2
let a:[i32;5] = [5,5,5,5,5]
//创建方式3
let a = [3;5] // [3,3,3,3,3]
```
## 函数
rust是基于表达式的语言。语句是一些执行操作但不返回值的指令，表达式计算并产生一个值。
**语句** 
```rust
let u:i32 = 6
```
**表达式** 
```rust
x+1
```

函数返回值需要使用箭头$->$声明返回值类型。可以使用return关键字和指定值从函数中返回，但是大部分函数隐式返回最后的表达式。
```rust
fn test()->i32{
    5 //此处不可加；，加上；后表示为一个语句，语句不返回值，因此此时会返回空单元元组，导致返回类型不匹配。
}
```
**函数返回多个值** 
```rust
fn addsub(x:usize, y:usize)->(usize,usize){ //通过返回元组，返回多个值
    (x+y,x-y)
}
```
如果函数需要返回多个类型，则可以将返回类型包裹在枚举中，接收函数根据结果分别进行处理。
```rust
use std::fs::File;
use std::io::BufReader;
use flate2::read::GzDecoder;
#[allow(non_camel_case_types)]
#[derive(Debug)]
pub enum return_type {
    gz(GzDecoder< BufReader<File>>),
    txt(BufReader<File>),
}
pub fn gzreader(file: &str) -> return_type {
    let fp = File::open(file).expect("open error");
    let gz_reader = GzDecoder::new(BufReader::new(fp));
    match gz_reader.header() {
        Some(_) => {
            return_type::gz(gz_reader)
        },
        None => {
            let fp =  File::open(file).expect("open error");
            let reader = BufReader::new(fp);
            return_type::txt(reader)
        },
    }
}
```

### 控制流
**if** 
rust并不会自动将非布尔值转换为布尔值，必须显式使用布尔值作为条件。
if语句是一个表达式，因此可以在let语句右侧使用它。
```rust
let number = if condation {5} else {6}; // 此时每个分支的返回值必须相同
```

**循环** 
rust存在三种循环：for,while,loop

```rust
// loop 使用技巧
// 1: loop返回值，将返回值加入用于停止循环的break表达式
let result = loop{
    counter += 1;
    if counter == 10{
        break counter * 2;
    }
};

// 2: loop label 指定break，continue退出的循环
'counting_up': loop{
    loop {
        if true{
            break 'counting_up';
        }
    }
}
```

for使用技巧
```rust
for number in (1..4).rev(){
    println!("{number}")
}
```

## rust所有权
它让 Rust 无需垃圾回收（garbage collector）即可保障内存安全。
三种管理运行时计算机内存的方式：
* 垃圾回收机制
* 程序员亲自分配释放
* 通过所有权系统管理内存，编译器在编译时根据一系列规则进行检查，违反了规则则程序不能编译。

所有权的主要目的是为了管理堆数据。
**所有权规则** 
1. Rust中每一个值都有一个所有者
2. 值在任一时刻有且只有一个所有者
3. 当所有者离开作用域，这个值将被丢弃

当离开作用域时，rust会自动调用drop函数，类似于RAII中的析构函数。

### 移动和克隆
**移动语义** 
rust中基本类型都是通过自动拷贝的方式进行赋值，然而对于复杂结构体，rust将通过转移所有权的方式。移动语义后，原变量变为未初始化状态，rust不允许使用未初始化的变量。Move语义实际上是进行了拷贝，但是对于结构复杂的类型，编译器会进行优化，通过转移地址提高效率。
```rust
#[derive(Debug)]
struct User{
    id:i32,
}
fn main(){
    let x:i32 = 123;
    let y:User=User { id: (x) };
    println!("x is {}",x);
    println!("y is {:?}",y);
    let t = y;
    println!("y is {:?}",y); // error borrow of moved value: `y`; move occurs because `y` has type `User`, which does not implement the `Copy` trait
    println!("t is {:?}",t);
}
```
![String所有权转移](https://kaisery.github.io/trpl-zh-cn/img/trpl04-04.svg)

String所有权转移


### Copy语义
在实现`Copy`后，结构体便具有了复制语义：
```rust
#[derive(Debug,Clone, Copy)]
struct User{
    id:i32,
}
fn main(){
    let x:i32 = 123;
    let y:User=User { id: (x) };
    println!("x is {}",x);
    println!("y is {:?}",y);
    let t = y;
    println!("y is {:?}",y);
    println!("t is {:?}",t);
}
```
Rust中默认实现了Copy Trait的类型，包括但不限于：
* 所有整数类型，比如u32
* 所有浮点数类型，比如f64
* 布尔类型，bool，它的值是true和false
* 字符类型，char
* 元组，当且仅当其包含的类型也都是Copy的时候。比如(i32, i32)是Copy的，但(i32, String)不是
* 共享指针类型或共享引用类型

可以手动实现`Copy`,方法如下：
```rust
// 方式1
#[derive(Copy, Clone)] //clone必须要有，它是Copy的父特征
struct Abc(i32, i32);

//方式2
impl Copy for Abc { }
impl Clone for Abc {
    fn clone(&self) -> Abc {
        *self
    }
}
```

**Copy 和 Clone 有什么区别？** 

复制是隐式发生的，例如作为分配 y = x 的一部分。Copy 的行为不可重载; 它始终是简单的按位复制。

克隆是一个明确的动作 x.clone()。Clone 的实现可以提供安全复制值所需的任何特定于类型的行为。 例如，用于 String的 Clone 的实现需要在堆中复制指向字符串的缓冲区。 String 值的简单按位副本将仅复制指针，从而导致该行向下双重释放。 因此，String是 Clone，但不是 Copy。

Clone 是 Copy 的父特征，因此 Copy 的所有类型也必须实现 Clone。 如果类型为 Copy，则其 Clone 实现仅需要返回 *self



## 泛型、Trait和生命周期
### Trait
