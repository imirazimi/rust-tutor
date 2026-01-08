# 📚 خلاصه کامل آموزش Rust

خلاصه کامل آموزش زبان برنامه‌نویسی Rust (از فایل rust-isf.pdf)

## 📋 فهرست مطالب

1. [مقدمه و نصب](#1-مقدمه-و-نصب)
2. [مبانی](#2-مبانی)
3. [توابع](#3-توابع)
4. [مالکیت و قرض‌گیری](#4-مالکیت-و-قرض-گیری)
5. [ساختارهای کنترلی](#5-ساختارهای-کنترلی)
6. [رشته‌ها](#6-رشته-ها)
7. [Structs و Enums](#7-structs-و-enums)
8. [مدیریت خطا](#8-مدیریت-خطا)
9. [Vector](#9-vector)
10. [impl و متدها](#10-impl-و-متدها)
11. [Closures](#11-closures)
12. [Iterators](#12-iterators)
13. [Traits](#13-traits)
14. [کار با فایل](#14-کار-با-فایل)
15. [Lifetimes](#15-lifetimes)
16. [برنامه‌نویسی ناهمزمان](#16-برنامه-نویسی-ناهمزمان)
17. [ماژول‌ها و کریت‌ها](#17-ماژول-ها-و-کریت-ها)

---

## 1. مقدمه و نصب

### چرا Rust؟
- **امنیت حافظه**: جلوگیری از خطاهای مربوط به حافظه بدون جمع‌آوری زباله
- **عملکرد بالا**: قابل مقایسه با C++
- **پشتیبانی از همزمانی**: برنامه‌نویسی همزمان ایمن
- **پشتیبانی از اشاره‌گرها**: استفاده ایمن از اشاره‌گرها

### نصب
```bash
# نصب rustup از https://www.rust-lang.org/tools/install
rustc --version  # بررسی نصب
```

### ابزارها
- **rustc**: کامپایلر Rust
- **cargo**: مدیر بسته و ابزار ساخت
- **VSCode**: ویرایشگر پیشنهادی با افزونه Rust (RLS)

### اولین برنامه
```rust
fn main() {
    println!("Hello, world!");
}
```

اجرا با: `cargo run`

---

## 2. مبانی

### متغیرها
```rust
let x = 5;              // Immutable by default
let mut y = 10;         // Mutable
y = 20;
```

### ثابت‌ها
```rust
const MAX_POINTS: u32 = 100_000;
static HELLO_WORLD: &str = "Hello, World";
```

### انواع داده

#### اعداد صحیح
- علامت‌دار: `i8`, `i16`, `i32`, `i64`, `i128`
- بدون علامت: `u8`, `u16`, `u32`, `u64`, `u128`

```rust
let x: i32 = 10;
let y: u64 = 20;
```

#### اعداد اعشاری
```rust
let a: f32 = 3.14;
let b: f64 = 2.71828;
```

#### کاراکتر
```rust
let ch: char = 'A';
```

#### بولین
```rust
let is_rust_fun: bool = true;
```

#### Tuple
```rust
let tup: (i32, f64, bool) = (42, 3.14, true);
let (a, b, c) = tup;
let x = tup.0;  // Access by index
```

#### آرایه‌ها
```rust
let arr: [i32; 5] = [1, 2, 3, 4, 5];
let first = arr[0];
```

### عملیات
```rust
let sum = 5 + 10;
let diff = 10 - 5;
let product = 5 * 2;
let quotient = 10 / 2;
```

### ورودی و خروجی
```rust
use std::io;

let mut name = String::new();
println!("Enter your name: ");
io::stdin().read_line(&mut name).expect("Failed to read line");
println!("Hello, {}", name);
```

---

## 3. توابع

### تابع ساده
```rust
fn greet(name: &str) {
    println!("Hello, {}!", name);
}
```

### تابع با نوع بازگشتی
```rust
fn add(a: i32, b: i32) -> i32 {
    a + b  // Last expression is returned automatically
}

// Or with explicit return
fn subtract(a: i32, b: i32) -> i32 {
    return a - b;
}
```

### توابع بدون بازگشت
```rust
fn print_message() {
    println!("This is a message");
    // Implicitly returns ()
}
```

### توابع inline
```rust
#[inline]
fn fast_add(a: i32, b: i32) -> i32 {
    a + b
}
```

---

## 4. مالکیت و قرض‌گیری

### قوانین مالکیت
1. هر مقدار یک مالک دارد
2. فقط یک مالک در هر زمان
3. وقتی مالک از scope خارج شود، مقدار drop می‌شود

### انواع Copy
```rust
let x = 5;
let y = x;  // x is still valid (copied)
println!("x: {}, y: {}", x, y);
```

### انواع Non-Copy (انتقال مالکیت)
```rust
let s1 = String::from("hello");
let s2 = s1;  // s1 is no longer valid (ownership moved)
// println!("{}", s1);  // ERROR!
println!("{}", s2);
```

### ارجاع‌ها (قرض گرفتن)
```rust
fn calculate_length(s: &String) -> usize {
    s.len()
}

let s3 = String::from("world");
let len = calculate_length(&s3);
println!("طول '{}' برابر {} است", s3, len);
```

### ارجاع‌های قابل تغییر
```rust
fn change(s: &mut String) {
    s.push_str(" world!");
}

let mut s4 = String::from("hello");
change(&mut s4);
println!("{}", s4);
```

### قوانین قرض‌گیری
1. می‌توانید تعداد نامحدود ارجاع تغییرناپذیر داشته باشید
2. فقط یک ارجاع قابل تغییر در هر زمان
3. نمی‌توانید همزمان ارجاع قابل تغییر و تغییرناپذیر داشته باشید

---

## 5. ساختارهای کنترلی

### if-else
```rust
let number = 7;
if number < 5 {
    println!("کمتر از 5");
} else {
    println!("بزرگتر یا مساوی 5");
}

// if as expression
let result = if number > 5 { "بزرگ" } else { "کوچک" };
```

### match
```rust
match number {
    1 => println!("یک"),
    2 | 3 => println!("دو یا سه"),
    4..=10 => println!("بین 4 تا 10"),
    _ => println!("چیز دیگری"),
}
```

### loop
```rust
let mut counter = 0;
let result = loop {
    counter += 1;
    if counter == 10 {
        break counter * 2;
    }
};
println!("نتیجه: {}", result);
```

### while
```rust
let mut number = 3;
while number != 0 {
    println!("{}", number);
    number -= 1;
}
```

### for
```rust
let arr = [10, 20, 30, 40, 50];
for element in arr.iter() {
    println!("مقدار: {}", element);
}

// Range
for number in 1..4 {
    println!("{}", number);
}

// با enumerate
for (index, &number) in arr.iter().enumerate() {
    println!("ایندکس: {}, مقدار: {}", index, number);
}
```

### continue و break
```rust
for i in 1..10 {
    if i % 2 == 0 {
        continue;  // Skip to next iteration
    }
    if i > 7 {
        break;  // Exit loop
    }
    println!("{}", i);
}
```

### حلقه‌های تو در تو با برچسب
```rust
'outer: for i in 1..4 {
    'inner: for j in 1..4 {
        if i == 2 && j == 2 {
            break 'outer;  // Break outer loop
        }
        println!("i: {}, j: {}", i, j);
    }
}
```

---

## 6. رشته‌ها

### String در مقابل &str
- **String**: مالکیت‌دار، قابل تغییر، ذخیره شده در heap
- **&str**: String slice، ارجاع تغییرناپذیر

### ایجاد رشته‌ها
```rust
let mut s = String::new();
let s1 = String::from("hello");
let s2 = "hello".to_string();
```

### تغییر رشته‌ها
```rust
let mut s = String::from("hello");
s.push_str(" world");
s.push('!');
```

### String Slices
```rust
let s1 = String::from("hello world");
let hello = &s1[0..5];      // "hello"
let world = &s1[6..11];    // "world"
let slice = &s1[..];       // Entire string
```

### String Literals
```rust
let s: &str = "hello";  // String literal is &str
```

### پیمایش روی رشته‌ها
```rust
// Characters
for c in "hello".chars() {
    println!("{}", c);
}

// Bytes
for b in "hello".bytes() {
    println!("{}", b);
}
```

### متدهای رایج رشته‌ها
```rust
let s = String::from("Hello World");
let new_s = s.replace("World", "Rust");
let trimmed = s.trim();
let contains = s.contains("Hello");
let starts = s.starts_with("Hello");
let ends = s.ends_with("World");
let lower = s.to_lowercase();
let upper = s.to_uppercase();
```

### تبدیل نوع
```rust
// To string
let number = 123;
let number_str = number.to_string();

// From string
let num_str = "42";
let num: i32 = num_str.parse().unwrap();
```

---

## 7. Structs و Enums

### Structs
```rust
struct User {
    username: String,
    email: String,
    age: u32,
}

let user1 = User {
    username: String::from("alice"),
    email: String::from("alice@example.com"),
    age: 25,
};

println!("کاربر: {}", user1.username);
```

### Enums
```rust
enum Direction {
    North,
    South,
    East,
    West,
}

let dir = Direction::North;
match dir {
    Direction::North => println!("شمال"),
    Direction::South => println!("جنوب"),
    Direction::East => println!("شرق"),
    Direction::West => println!("غرب"),
}
```

### Enums با داده
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

let msg = Message::Move { x: 10, y: 20 };
match msg {
    Message::Quit => println!("خروج"),
    Message::Move { x, y } => println!("حرکت به x: {}, y: {}", x, y),
    Message::Write(text) => println!("نوشتن: {}", text),
    Message::ChangeColor(r, g, b) => println!("رنگ: r:{}, g:{}, b:{}", r, g, b),
}
```

### Option<T>
```rust
let some_number: Option<i32> = Some(5);
let no_number: Option<i32> = None;

match some_number {
    Some(value) => println!("مقدار: {}", value),
    None => println!("مقداری وجود ندارد"),
}
```

### Match غیر جامع
```rust
// ERROR: Not all cases covered
match msg {
    Message::Write(text) => println!("{}", text),
    // Missing other variants - compiler error!
}

// Solution: Use wildcard
match msg {
    Message::Write(text) => println!("{}", text),
    _ => println!("نوع پیام دیگر"),
}
```

---

## 8. مدیریت خطا

### Option<T>
```rust
fn find_user(id: u32) -> Option<String> {
    if id == 1 {
        Some(String::from("alice"))
    } else {
        None
    }
}

match find_user(1) {
    Some(name) => println!("پیدا شد: {}", name),
    None => println!("پیدا نشد"),
}
```

### Result<T, E>
```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("تقسیم بر صفر امکان‌پذیر نیست!"))
    } else {
        Ok(a / b)
    }
}

match divide(10.0, 2.0) {
    Ok(result) => println!("نتیجه: {}", result),
    Err(e) => println!("خطا: {}", e),
}
```

### unwrap و expect
```rust
// DANGEROUS: Will panic on error
let result = divide(10.0, 2.0).unwrap();
let result = divide(10.0, 0.0).expect("Division failed!");
```

### عملگر ?
```rust
fn perform_division(a: f64, b: f64, c: f64) -> Result<f64, String> {
    let div1 = divide(a, b)?;  // If Err, return immediately
    let div2 = divide(div1, c)?;  // If Err, return immediately
    Ok(div1 + div2)
}
```

---

## 9. Vector

### ایجاد Vector
```rust
let mut v: Vec<i32> = Vec::new();
v.push(1);
v.push(2);
v.push(3);

// Or with macro
let v2 = vec![1, 2, 3, 4, 5];
```

### دسترسی به عناصر
```rust
let third = &v2[2];  // Panics if index out of bounds

// Safe access
match v2.get(2) {
    Some(value) => println!("مقدار: {}", value),
    None => println!("ایندکس خارج از محدوده"),
}
```

### حذف عناصر
```rust
let mut v = vec![1, 2, 3, 4, 5];
let last = v.pop();  // Returns Option<T>
let removed = v.remove(1);  // Removes at index, panics if invalid
```

### پیمایش
```rust
// قرض تغییرناپذیر
for i in &v2 {
    println!("{}", i);
}

// قرض قابل تغییر
let mut v3 = vec![1, 2, 3];
for i in &mut v3 {
    *i += 10;
}

// مالکیت (مصرف می‌کند vector)
for i in v3 {
    println!("{}", i);
}
// v3 دیگر اینجا معتبر نیست
```

### متدهای مفید
```rust
let v = vec![1, 2, 3, 4, 5];
println!("طول: {}", v.len());
println!("ظرفیت: {}", v.capacity());
```

### ذخیره انواع مختلف با Enum
```rust
enum Cell {
    Integer(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    Cell::Integer(99),
    Cell::Text(String::from("example")),
    Cell::Float(15.5),
];
```

### مالکیت با Vector
```rust
let mut v = vec![1, 2, 3];
let first = &v[0];  // قرض تغییرناپذیر
// v.push(6);  // خطا: نمی‌تواند به صورت قابل تغییر قرض بگیرد
println!("{}", first);
v.push(6);  // OK بعد از اینکه first از scope خارج شد
```

---

## 10. impl و متدها

### متدها برای Structs
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // متد با &self
    fn area(&self) -> u32 {
        self.width * self.height
    }
    
    // متد با &mut self
    fn set_width(&mut self, width: u32) {
        self.width = width;
    }
    
    // متد با self (مالکیت می‌گیرد)
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}

let rect = Rectangle { width: 30, height: 50 };
println!("مساحت: {}", rect.area());
```

### توابع مرتبط
```rust
impl Rectangle {
    // تابع مرتبط (مثل constructor)
    fn new(width: u32, height: u32) -> Rectangle {
        Rectangle { width, height }
    }
    
    fn square(size: u32) -> Rectangle {
        Rectangle { width: size, height: size }
    }
}

let rect = Rectangle::new(30, 50);
let square = Rectangle::square(10);
```

### متدها برای Enums
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}

impl Message {
    fn call(&self) {
        match self {
            Message::Quit => println!("خروج"),
            Message::Move { x, y } => println!("حرکت به ({}, {})", x, y),
            Message::Write(text) => println!("نوشتن: {}", text),
        }
    }
}

let msg = Message::Write(String::from("hello"));
msg.call();
```

---

## 11. Closures

### سینتکس پایه
```rust
let add_one = |x: i32| -> i32 { x + 1 };
let result = add_one(5);

// استنتاج نوع
let add = |x, y| x + y;
let result = add(3, 4);
```

### Closures و محیط
```rust
let x = 4;
let equal_to_x = |z| z == x;  // Closure captures x
println!("{}", equal_to_x(4));
```

### انواع Closure
```rust
// Fn: قرض تغییرناپذیر
let fn_closure = |x| x + 1;

// FnMut: قرض قابل تغییر
let mut count = 0;
let mut fnmut_closure = || {
    count += 1;
    count
};

// FnOnce: مالکیت می‌گیرد
let fnonce_closure = move || {
    let owned_string = String::from("hello");
    owned_string
};
```

---

## 12. Iterators

### Iterator پایه
```rust
let v = vec![1, 2, 3];
let mut iter = v.iter();

assert_eq!(iter.next(), Some(&1));
assert_eq!(iter.next(), Some(&2));
assert_eq!(iter.next(), Some(&3));
assert_eq!(iter.next(), None);
```

### متدهای Iterator

#### map
```rust
let v = vec![1, 2, 3];
let doubled: Vec<i32> = v.iter().map(|x| x * 2).collect();
```

#### filter
```rust
let v = vec![1, 2, 3, 4, 5];
let evens: Vec<&i32> = v.iter().filter(|x| *x % 2 == 0).collect();
```

#### collect
```rust
let v: Vec<i32> = (1..5).collect();
```

#### take(n)
```rust
let first_three: Vec<i32> = (1..10).take(3).collect();
```

#### skip(n)
```rust
let skipped: Vec<i32> = (1..10).skip(3).collect();
```

#### enumerate()
```rust
for (index, value) in vec![10, 20, 30].iter().enumerate() {
    println!("ایندکس: {}, مقدار: {}", index, value);
}
```

### زنجیره‌سازی
```rust
let result: Vec<i32> = (1..100)
    .filter(|x| x % 2 == 0)
    .map(|x| x * 2)
    .take(5)
    .collect();
```

---

## 13. Traits

### تعریف Trait
```rust
trait Summary {
    fn summarize(&self) -> String;
}
```

### پیاده‌سازی Trait
```rust
struct NewsArticle {
    headline: String,
    location: String,
    author: String,
    content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}
```

### استفاده از Traits به عنوان پارامتر
```rust
fn notify(item: &impl Summary) {
    println!("خبر فوری! {}", item.summarize());
}

// یا با trait bounds
fn notify<T: Summary>(item: &T) {
    println!("خبر فوری! {}", item.summarize());
}
```

### مثال: اشکال هندسی
```rust
trait Shape {
    fn area(&self) -> f64;
    fn perimeter(&self) -> f64;
}

struct Circle {
    radius: f64,
}

impl Shape for Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * self.radius * self.radius
    }
    
    fn perimeter(&self) -> f64 {
        2.0 * std::f64::consts::PI * self.radius
    }
}

struct Rectangle {
    width: f64,
    height: f64,
}

impl Shape for Rectangle {
    fn area(&self) -> f64 {
        self.width * self.height
    }
    
    fn perimeter(&self) -> f64 {
        2.0 * (self.width + self.height)
    }
}
```

---

## 14. کار با فایل

### خواندن فایل
```rust
use std::fs;

let contents = fs::read_to_string("hello.txt")
    .expect("Something went wrong reading the file");
println!("{}", contents);
```

### نوشتن فایل
```rust
use std::fs;

let data = "Hello, Rust!";
fs::write("output.txt", data)
    .expect("Something went wrong writing the file");
```

### مثال: پایگاه داده ساده با HashMap
```rust
use std::collections::HashMap;
use std::fs;

struct Database {
    data: HashMap<String, String>,
}

impl Database {
    fn new() -> Database {
        Database {
            data: HashMap::new(),
        }
    }
    
    fn set(&mut self, key: String, value: String) {
        self.data.insert(key, value);
    }
    
    fn get(&self, key: &str) -> Option<&String> {
        self.data.get(key)
    }
}
```

---

## 15. Lifetimes

### طول عمر استنباط شده توسط کامپایلر
```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    &s[..]
}
```

### مشخص کردن صریح طول عمر
```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

### طول عمر 'static
```rust
let s: &'static str = "I have a static lifetime.";
```

### طول عمر در Structs
```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

---

## 16. برنامه‌نویسی ناهمزمان

### Future
```rust
use std::future::Future;

async fn fetch_data() -> String {
    String::from("Data")
}
```

### async/await
```rust
async fn process_data() {
    let data = fetch_data().await;
    println!("{}", data);
}
```

### Executor
```rust
// استفاده از runtime tokio
#[tokio::main]
async fn main() {
    let result = fetch_data().await;
    println!("{}", result);
}
```

### spawn برای اجرای مستقل
```rust
use tokio::task;

task::spawn(async {
    println!("Running in background");
});
```

---

## 17. ماژول‌ها و کریت‌ها

### کریت چیست؟
- **کریت اجرایی**: برنامه قابل اجرا
- **کریت کتابخانه**: کد قابل استفاده مجدد

### ماژول چیست؟
ماژول‌ها کد را درون یک کریت سازماندهی می‌کنند

### تعریف ماژول‌ها
```rust
mod math {
    pub mod basic {
        pub fn add(a: i32, b: i32) -> i32 {
            a + b
        }
        
        fn subtract(a: i32, b: i32) -> i32 {
            a - b  // Private
        }
    }
}

// Using with use keyword
use math::basic::add;

fn main() {
    // Absolute path
    println!("{}", crate::math::basic::add(5, 3));
    
    // With use
    println!("{}", add(5, 3));
}
```

### قابلیت مشاهده با pub
```rust
mod outer_module {
    pub mod inner_module {
        pub fn public_function() {
            println!("This is public!");
        }
        
        fn private_function() {
            println!("This is private.");
        }
    }
}
```

### مسیرها
```rust
// Absolute path
crate::math::basic::add(5, 3);

// Relative path
self::math::basic::add(5, 3);
super::some_function();
```

### وارد کردن چندگانه
```rust
use std::collections::{HashMap, HashSet};
```

### نام مستعار
```rust
use std::fmt::Result;
use std::io::Result as IoResult;
```

### کریت‌های خارجی
```toml
# Cargo.toml
[dependencies]
rand = "0.8.5"
```

```rust
use rand::Rng;

fn main() {
    let mut rng = rand::thread_rng();
    let random_number = rng.gen_range(1..=100);
    println!("عدد تصادفی: {}", random_number);
}
```

### سازماندهی فایل‌ها
- `src/main.rs`: ریشه کریت اجرایی
- `src/lib.rs`: ریشه کریت کتابخانه
- `src/mod_name.rs`: کد ماژول
- `src/mod_name/mod.rs`: سازماندهی جایگزین ماژول

---

## 📝 خلاصه قوانین مهم

### قوانین مالکیت
1. هر مقدار یک مالک دارد
2. فقط یک مالک در هر زمان
3. وقتی مالک از scope خارج شود، مقدار drop می‌شود

### قوانین قرض‌گیری
1. می‌توانید تعداد نامحدود ارجاع تغییرناپذیر داشته باشید
2. فقط یک ارجاع قابل تغییر در هر زمان
3. نمی‌توانید همزمان ارجاع قابل تغییر و تغییرناپذیر داشته باشید

### Pattern Matching
- `match` باید جامع باشد (همه حالات را پوشش دهد)
- از `_` برای catch-all استفاده کنید

### مدیریت خطا
- از `unwrap()` فقط برای پروتوتایپ استفاده کنید
- در کد production از `match` یا عملگر `?` استفاده کنید

---

## 🎯 مسیر یادگیری

### هفته 1: مبانی
- روز 1-2: نصب و Hello World
- روز 3-4: متغیرها و انواع داده
- روز 5: توابع
- روز 6-7: مالکیت (بسیار مهم!)

### هفته 2: متوسط
- روز 8-9: ساختارهای کنترلی
- روز 10: رشته‌ها
- روز 11-12: Structs و Enums
- روز 13-14: مدیریت خطا

### هفته 3: پیشرفته
- روز 15: Vector
- روز 16: impl و متدها
- روز 17-18: Closures و Iterators
- روز 19-20: Traits
- روز 21-22: کار با فایل و Lifetimes
- روز 23-24: برنامه‌نویسی ناهمزمان
- روز 25-26: ماژول‌ها و کریت‌ها

---

## 💡 پروژه‌های تمرینی

1. ماشین حساب ساده با توابع
2. مدیریت لیست کارها با Vector
3. سیستم کاربر با Struct
4. بازی حدس عدد با `match` و `Result`
5. کتابخانه ریاضی با ماژول‌ها
6. پایگاه داده مبتنی بر فایل با HashMap
7. Web scraper ناهمزمان
8. برنامه CLI با مدیریت خطا

---

## 🔗 منابع اضافی

- [The Rust Book](https://doc.rust-lang.org/book/)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Rustlings Exercises](https://github.com/rust-lang/rustlings)
- [Rust API Documentation](https://doc.rust-lang.org/std/)
