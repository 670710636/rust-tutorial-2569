# Rust Tutorial Project — Principles of Programming Languages

> **กลุ่มที่:** 18
> **Topic No.:** 18
> **Topic Name:** Modules, Packages & Crates
> **ประเด็นหลักที่ควรครอบคลุม:** module, visibility, pub, package, crate, use, project organization

---

## 1. Members

| # | Name | Student ID | GitHub Username | Main Responsibility |
|---|---|---|---|---|
| 1 | นายสิปปกร ทองนุ่ม | 670710635 | `@[670710635]` | Concept + Short Code Illustration (สรุปแนวคิดหลัก + โค้ดตัวอย่างสั้น) |
| 2 | นางสาวอชิรญา ก้อนสุวรรณ | 670710636 | `@[670710636]` | Detailed Code + Live Demo (โค้ดเชิงลึก + สาธิตสด) |
| 3 | นางสาวอนัญญณัชช์ ขจรศิริผล | 670710637 | `@[670710637]` | Rust vs Other Language + PPL Analysis (เปรียบเทียบภาษา + วิเคราะห์เชิง PPL) |
| 4 | นายกฤติน เหลืองระลึก | 670710638 | `@[670710638]` | Exercises + Common Mistakes + Challenge (แบบฝึกหัด + ข้อผิดพลาดที่พบบ่อย + คำถามท้าทาย) |

> แก้ไข GitHub Username ของแต่ละคนให้ตรงกับบัญชีจริงก่อนเริ่มทำงาน (ผู้สอนจะใช้คอลัมน์นี้เชิญเป็น collaborator ของ repository)

---

## 2. Learning Objectives

หลังจากศึกษา Topic นี้แล้ว ผู้เรียนสามารถ:

1. `[อธิบายแนวคิดสำคัญได้]`
2. `[เขียนโปรแกรม Rust ที่เกี่ยวข้องได้]`
3. `[วิเคราะห์พฤติกรรม/กฎของภาษาได้]`
4. `[เปรียบเทียบ Rust กับภาษาอื่นได้]`

---

## 3. Introduction

`[เขียนเนื้อหาที่นี่ — ใช้โครงสร้างเดียวกับ rust_tutorial_template.md ฉบับเต็มที่ผู้สอนแจกให้]`

---

*โครงสร้างเอกสารฉบับเต็ม (Key Concepts, Runnable Code Examples, Common Mistakes, Exercises, PPL Perspective, Rust vs Other Language, References, AI Usage Declaration, GitHub Contribution, Final Checklist) ให้ทำต่อจากจุดนี้ตาม Template หลักของวิชา (`rust_tutorial_template.md`) ที่แนบมากับใบมอบหมายงาน*

---

## 6.Runable Code Example

ตัวอย่างต่อไปนี้แสดงการทำงานของ Modules, Visibility, `pub`, `use`,
Package, Crate และการจัดโครงสร้างโปรเจกต์ในภาษา Rust

ตัวอย่างทั้งหมดใช้โปรเจกต์ `modules_demo` และสามารถนำไปทดลองรัน
เพื่อดูผลลัพธ์ได้จริง

### Example 1 — Module, Visibility และ `pub`

ตัวอย่างนี้แสดงวิธีสร้าง Module และกำหนดว่า Function ใดสามารถ
ถูกเรียกใช้งานจากภายนอก Module ได้

#### Code ตัวอย่าง

```rust
mod calculator {
    pub fn add(a: i32, b: i32) -> i32 {
        a + b
    }

    fn secret_operation(a: i32, b: i32) -> i32 {
        a * b
    }
}

fn main() {
    let result = calculator::add(10, 20);

    println!("Result = {}", result);
}
```
ผลลัพธ์ที่คาดว่าจะได้
```
Result = 30
```
#### โดยอธิบายทีละส่วนดังนี้

คำสั่ง *mod*
```rust
mod calculator
```
ใช้สำหรับสร้าง Module ที่ชื่อว่า `calculator`

Function `add` ถูกประกาศด้วย `pub`
```rust
pub fn add(a: i32, b: i32) -> i32
```
คำว่า `pub` ย่อมาจาก public และทำให้ Function นี้สามารถถูกเรียกใช้งานจากภายนอก Module ได้

ในทางตรงกันข้าม Function secret_operation ไม่ได้ใช้ pub
```rust
fn secret_operation(a: i32, b: i32) -> i32
```
ดังนั้น Function นี้จะเป็น private และสามารถใช้งานได้ภายในModule calculator เท่านั้น

โดยการเรียกใช้งาน Function สามารถเขียนเป็น
```rust
calculator::add(10, 20)
```
โดย `::` ใช้สำหรับเข้าถึงสิ่งที่อยู่ภายใน Module ผ่าน Module Path

#### ซึ่งตัวอย่างที่ 1 นี้แสดงแนวคิดเรื่อง Visibility ของ Rust:

- `pub` → สามารถเข้าถึงจากภายนอก Module ได้
- ไม่มี `pub` → เป็น private โดยค่าเริ่มต้น
- `::` → ใช้เข้าถึงสิ่งต่าง ๆ ผ่าน Module Path

#### ภาพรวมของ Example 1

```text
calculator
│
├── pub add()
│      ↑
│      └── main สามารถเรียกใช้งานได้
│
└── secret_operation()
       ↑
       └── เป็น private
```

### Example 2 — การใช้ `use` กับหลาย Module

ตัวอย่างนี้แสดงการแบ่งโปรแกรมออกเป็นหลาย Module
และการใช้คำสั่ง `use` เพื่อนำ Function จาก Module อื่นมาใช้งาน

#### โครงสร้างโปรเจกต์

```text
modules_demo/
├── main.rs
├── calculator.rs
└── utils.rs
```

ในตัวอย่าง Project `modules_demo` นี้แบ่ง Code ออกเป็น 3 ไฟล์

- **main.rs** → โปรแกรมหลัก
- **calculator.rs** → Function สำหรับการคำนวณ
- **utils.rs** → Function สำหรับงานทั่วไป

#### calculator.rs
```rust
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

pub fn subtract(a: i32, b: i32) -> i32 {
    a - b
}

pub fn multiply(a: i32, b: i32) -> i32 {
    a * b
}

pub fn divide(a: i32, b: i32) -> f64 {
    a as f64 / b as f64
}
```

#### utils.rs
```rust
pub fn print_title(title: &str) {
    println!("====================");
    println!("{}", title);
    println!("====================");
}
```

#### main.rs
```rust
mod calculator;
mod utils;

use calculator::{add, subtract, multiply, divide};
use utils::print_title;

fn main() {
    print_title("Rust Modules Demo");

    let a = 20;
    let b = 10;

    println!("Addition: {}", add(a, b));
    println!("Subtraction: {}", subtract(a, b));
    println!("Multiplication: {}", multiply(a, b));
    println!("Division: {}", divide(a, b));
}
```

ผลลัพธ์ที่คาดว่าจะได้
```
====================
Rust Modules Demo
====================
Addition: 30
Subtraction: 10
Multiplication: 200
Division: 2
```

### สาธิตการทำงานของแนวคิดทั้งหมดร่วมกันโดยใช้โปรเจกต์ `modules_demo`
#### ขั้นตอนที่ 1: ตรวจสอบโครงสร้างโปรเจกต์
```text
modules_demo/
├── main.rs
├── calculator.rs
└── utils.rs
```

โปรเจกต์นี้ถูกแบ่งออกเป็น 3 Module เพื่อให้แต่ละส่วนรับผิดชอบหน้าที่ที่แตกต่างกัน
- **main.rs** → โปรแกรมหลัก
- **calculator** → เก็บ Function ที่เกี่ยวข้องกับการคำนวณ
- **utils** → เก็บ Function สำหรับงานทั่วไป

#### ขั้นตอนที่ 2: ตรวจสอบ Module ใน `main.rs`
คำสั่ง
```rust
mod calculator;
mod utils;
```
ใช้เพื่อบอก Rust ว่าโปรแกรมนี้มี Module ที่ชื่อว่า `calculator` และ `utils`

#### ขั้นตอนที่ 3: นำ Function มาใช้งานด้วย `use`
```rust
use calculator::{add, subtract, multiply, divide};
use utils::print_title;
```
เพื่อนำ Function ที่ต้องการมาใช้งานใน Scope ปัจจุบัน
เมื่อเรียก Function มาใช้งานโดยตรง สามารถเขียนสั้นลงเป็น
```rust
add(20, 10);
```
แทนที่จะต้องเขียน
```rust
calculator::add(20, 10);
```

#### ขั้นตอนที่ 4: ตรวจสอบ Visibility
Function ใน `calculator.rs` ถูกประกาศด้วย `pub`
```rust
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```
ทำให้ Function สามารถถูกเรียกใช้งานจาก Module อื่นได้

#### ขั้นตอนที่ 5: รันโปรแกรม
ใช้คำสั่งบน Terminal ในโฟลเดอร์ของโปรเจกต์
```rust
cargo run
```

#### สรุปการทำงาน
Demo นี้แสดงให้เห็นว่า
1. `mod` ใช้ประกาศ Module
2. `pub` ใช้กำหนดให้ Function สามารถเข้าถึงจากภายนอก Module ได้
3. `use` ช่วยนำ Function หรือ Item มาใช้งานได้สะดวกขึ้น
4. Module ช่วยแบ่ง Code ออกเป็นส่วน ๆ
5. Crate คือหน่วยของ Source Code ที่นำไป Compile
6. Package คือโปรเจกต์ที่ Cargo ใช้จัดการ
7. การแบ่ง Code เป็นหลายไฟล์ช่วยให้โปรเจกต์มีระเบียบและดูแลได้ง่าย

การแบ่ง Function ออกเป็น Module ทำให้ Code มีระเบียบมากขึ้น อ่านง่ายขึ้น และสามารถดูแลหรือแก้ไขในอนาคตได้ง่ายขึ้น
#### จำง่าย ๆ แบบนี้
```text
PACKAGE
│
│  โปรเจกต์ที่ Cargo จัดการ
│
└── CRATE
    │
    │  หน่วย Code ที่ Compiler Compile
    │
    ├── MODULE
    │   └── calculator
    │
    └── MODULE
        └── utils
```

---
## 7. Common Mistakes

### Mistake 1 — ลืมใส่ `pub` ให้ฟังก์ชันใน module

**Problem**

ใน Rust ทุก item (ฟังก์ชัน, struct, enum, module ฯลฯ) เป็น **private โดยค่าเริ่มต้น** ถ้าอยู่ใน module หนึ่งแล้วต้องการให้โค้ดข้างนอก module เรียกใช้ ต้องเติม `pub` เอง ผู้เริ่มต้นที่มาจาก Python มักลืม เพราะ Python ไม่มีการบังคับ private จริง ๆ

**Incorrect Code**

```rust
mod bank {
    fn deposit(balance: u32, amount: u32) -> u32 {   // ❌ private
        balance + amount
    }
}

fn main() {
    println!("{}", bank::deposit(100, 50));
}
```

Compiler แจ้ง error `E0603: function 'deposit' is private`

**Correct Code**

```rust
mod bank {
    pub fn deposit(balance: u32, amount: u32) -> u32 {   // ✅ เติม pub
        balance + amount
    }
}

fn main() {
    println!("{}", bank::deposit(100, 50));
}
```

Output: `150`

**Why?**

Rust ออกแบบให้ **encapsulation เป็นค่าเริ่มต้น** (private by default) ผู้เขียน module ต้องตัดสินใจอย่างชัดเจนว่าจะเปิดอะไรให้ภายนอกใช้ ทำให้ interface ของ module เล็กและแก้ implementation ภายในได้โดยไม่กระทบโค้ดที่เรียกใช้ การตรวจสอบนี้เกิดตอน compile time ไม่ใช่ runtime

---

### Mistake 2 — ประกาศ `pub struct` แต่ลืมว่า field ยัง private

**Problem**

การใส่ `pub` หน้า struct ทำให้ **ชื่อ struct** มองเห็นได้ แต่ **field แต่ละตัวยังเป็น private** ต้องใส่ `pub` ที่ field เอง หรือ (ที่ดีกว่าในหลายกรณี) ให้ module มี constructor และ getter ให้ ผลคือสร้าง struct ด้วย literal จากข้างนอกไม่ได้

**Incorrect Code**

```rust
mod bank {
    pub struct Account {
        pub owner: String,
        balance: u32,               // private
    }
}

fn main() {
    let acc = bank::Account {
        owner: String::from("Somchai"),
        balance: 100,               // ❌ E0451: field is private
    };
}
```

**Correct Code**

```rust
mod bank {
    pub struct Account {
        pub owner: String,
        balance: u32,               // ยัง private เหมือนเดิม
    }

    impl Account {
        pub fn new(owner: &str, balance: u32) -> Account {
            Account {
                owner: String::from(owner),
                balance,
            }
        }

        pub fn balance(&self) -> u32 {
            self.balance
        }
    }
}

fn main() {
    let acc = bank::Account::new("Somchai", 100);
    println!("{} has {}", acc.owner, acc.balance());
}
```

Output: `Somchai has 100`

**Why?**

`pub` ใน Rust ควบคุมทีละระดับ (struct แยกจาก field) ทำให้ผู้ออกแบบ module รักษา **invariant** ของข้อมูลได้ เช่น ห้ามให้ใครแก้ `balance` ตรง ๆ ต้องผ่านเมธอดที่ตรวจเงื่อนไขก่อน การอ่านค่า field ที่ private จากข้างนอกจะเกิด error `E0616` ในทำนองเดียวกัน

---

### Mistake 3 — สร้างไฟล์ `.rs` แล้วคิดว่า Rust จะรู้จักเอง (ลืม `mod`)

**Problem**

Rust **ไม่ได้ค้นหาไฟล์ในโฟลเดอร์อัตโนมัติ** การสร้างไฟล์ `src/utils.rs` เฉยๆ ไม่ทำให้มันเป็นส่วนหนึ่งของ crate ต้องประกาศ `mod utils;` ใน crate root (`main.rs` หรือ `lib.rs`) ก่อน คนที่มาจาก Python/Java มักเข้าใจว่าไฟล์ = module โดยอัตโนมัติ

**โครงสร้างโปรเจกต์**

```text
my_app/
├── Cargo.toml
└── src/
    ├── main.rs
    └── utils.rs
```

`src/utils.rs`

```rust
pub fn greet() {
    println!("สวัสดีจาก utils");
}
```

**Incorrect Code** (`src/main.rs`)

```rust
use crate::utils::greet;    // ❌ E0432: unresolved import (ยังไม่มี mod utils)

fn main() {
    greet();
}
```

**Correct Code** (`src/main.rs`)

```rust
mod utils;                  // ✅ บอก compiler ว่า utils เป็นส่วนของ crate นี้
use utils::greet;

fn main() {
    greet();
}
```

Output: `สวัสดีจาก utils`

**Why?**

ใน Rust **module tree ถูกสร้างจากการประกาศ `mod`** ไม่ใช่จากโครงสร้างไฟล์ คำสั่ง `mod utils;` บอก compiler ให้ไปอ่านโค้ดจาก `utils.rs` (หรือ `utils/mod.rs`) มาใส่ไว้เป็น module ชื่อ `utils` ส่วน `use` เป็นเพียงการ **ตั้งชื่อลัด** ให้ path ที่มีอยู่แล้ว ไม่ได้สร้าง module ขึ้นมา (`mod` = สร้าง/ประกาศ, `use` = นำเข้าชื่อ)

---

### Mistake 4 — คิดว่า module ลูก "เห็น" ชื่อของ module แม่โดยอัตโนมัติ

**Problem**

แต่ละ module มี **scope ของตัวเอง** ชื่อที่ประกาศใน module แม่ไม่ได้ถูกนำเข้ามาใน module ลูกอัตโนมัติ (ต่างจาก nested function ในบางภาษา) ต้องอ้างผ่าน path เช่น `super::` หรือ `crate::`

**Incorrect Code**

```rust
fn helper() -> u32 {
    42
}

mod inner {
    pub fn run() -> u32 {
        helper()                 // ❌ E0425: cannot find function `helper` in this scope
    }
}

fn main() {
    println!("{}", inner::run());
}
```

**Correct Code**

```rust
fn helper() -> u32 {
    42
}

mod inner {
    pub fn run() -> u32 {
        super::helper()          // ✅ super = module แม่ (ในที่นี้คือ crate root)
    }
}

fn main() {
    println!("{}", inner::run());
}
```

Output: `42`

**Why?**

Rust ใช้ **module เป็นขอบเขตของ namespace** ชื่อไม่ไหลข้ามขอบเขตโดยอัตโนมัติ (ผู้อ่านโค้ดจึงเห็นชัดว่าแต่ละ module พึ่งพาอะไร) สังเกตว่า `helper` เป็น private แต่ module ลูกยังเรียกได้ เพราะกฎ privacy คือ *"item เป็น private = มองเห็นได้เฉพาะใน module ที่ประกาศมัน **และ module ลูกหลานของมัน**"*

---
### สรุป Error Code ที่พบบ่อยในหัวข้อนี้

| Error Code | ความหมาย | สาเหตุที่พบบ่อย |
|---|---|---|
| `E0603` | item เป็น private | ลืม `pub` |
| `E0451` | field ของ struct เป็น private (ตอนสร้าง struct literal) | ลืม `pub` ที่ field |
| `E0616` | อ่าน field ที่เป็น private | เข้าถึง field ตรง ๆ จากนอก module |
| `E0432` | import ไม่พบ | ลืม `mod` หรือพิมพ์ path ผิด |
| `E0425` | หาชื่อฟังก์ชัน/ตัวแปรไม่พบใน scope | ลืม `super::` / `crate::` / `use` |

---

## 8. Exercises

> จัดทำแบบฝึกหัด **2 ข้อ** ที่สอดคล้องกับ Topic และมีระดับความยากเหมาะสม

### Exercise 1 — สร้าง Module ซ้อนกัน (ระดับพื้นฐาน)

**Problem**

เขียนโปรแกรมไฟล์เดียวที่มี module `shapes` ซึ่งมี **module ย่อย 2 ตัว** คือ `circle` และ `rectangle` โดยแต่ละตัวมีฟังก์ชัน `area` ที่คำนวณพื้นที่ แล้วเรียกใช้จาก `main` ด้วย `use` โดยต้องตั้งชื่อลัด (`as`) ให้ `rectangle::area` เพื่อไม่ให้ชื่อชนกัน

ผลลัพธ์ที่ต้องได้:

```text
Circle (r=2): 12.57
Rectangle (3x4): 12.00
```

**Hint**

- module ย่อยที่อยู่ใน module `shapes` ต้องเป็น `pub mod` และฟังก์ชันข้างในต้องเป็น `pub fn`
- ค่า π ใช้ `std::f64::consts::PI`
- ทศนิยม 2 ตำแหน่งใช้ `{:.2}`
- การตั้งชื่อลัด: `use shapes::rectangle::area as rect_area;`

**Solution**

```rust
mod shapes {
    pub mod circle {
        pub fn area(r: f64) -> f64 {
            std::f64::consts::PI * r * r
        }
    }

    pub mod rectangle {
        pub fn area(w: f64, h: f64) -> f64 {
            w * h
        }
    }
}

use shapes::circle;
use shapes::rectangle::area as rect_area;

fn main() {
    println!("Circle (r=2): {:.2}", circle::area(2.0));
    println!("Rectangle (3x4): {:.2}", rect_area(3.0, 4.0));
}
```

**Explanation**

1. `mod shapes { ... }` สร้าง module แม่ และข้างในซ้อน `pub mod circle` / `pub mod rectangle` (ต้องมี `pub` เพราะ `main` อยู่นอก `shapes`)
2. ทั้งสอง module มีฟังก์ชันชื่อ `area` เหมือนกันได้ เพราะแยก namespace กัน (`shapes::circle::area` กับ `shapes::rectangle::area` เป็นคนละชื่อเต็ม)
3. `use shapes::circle;` นำเข้า **module** มาใช้ เรียกเป็น `circle::area(...)` ซึ่งอ่านง่ายกว่านำเข้าฟังก์ชันตรง ๆ (แนวทางที่ Rust Book แนะนำ)
4. `use ... as rect_area` ตั้งชื่อลัด กันไม่ให้ชื่อ `area` สองตัวชนกันใน scope ของ `main`
5. พื้นที่วงกลม = π × 2² ≈ 12.57 และพื้นที่สี่เหลี่ยม = 3 × 4 = 12.00

---

### Exercise 2 — สร้าง Package ที่มี Library Crate + Binary Crate (ระดับกลาง)

**Problem**

ใช้ Cargo สร้าง package ชื่อ `thai_greeter` ที่ประกอบด้วย

1. **library crate** (`src/lib.rs`) ที่มี `pub mod greeting;`
2. ไฟล์ `src/greeting.rs` ที่มีฟังก์ชัน **private** `decorate` และฟังก์ชัน **public** `hello(name: &str) -> String`, โดย `hello` ต้องเรียกใช้ `decorate`
3. **binary crate** (`src/main.rs`) ที่เรียก `hello("Rust")` จาก library แล้วพิมพ์ผล

จากนั้นตอบคำถาม 2 ข้อ

- ก. package นี้มีกี่ crate และเป็นชนิดอะไรบ้าง
- ข. ถ้าลองให้ `main.rs` เรียก `thai_greeter::greeting::decorate(...)` ตรง ๆ จะเกิดอะไรขึ้น เพราะอะไร

ผลลัพธ์ของ `cargo run`:

```text
*** สวัสดี Rust ***
```

**Hint**

- สร้างด้วย `cargo new thai_greeter --lib` จะได้ `src/lib.rs` ให้ แล้วสร้าง `src/main.rs` เพิ่มเอง Cargo จะถือว่ามี binary crate ให้อัตโนมัติ
- `main.rs` อ้างถึง library ด้วยชื่อ package (ขีดกลางแปลงเป็นขีดล่าง) เช่น `use thai_greeter::greeting::hello;`
- `decorate` ไม่ต้องใส่ `pub`

**Solution**

`Cargo.toml`

```toml
[package]
name = "thai_greeter"
version = "0.1.0"
edition = "2021"
```

`src/lib.rs`

```rust
pub mod greeting;
```

`src/greeting.rs`

```rust
fn decorate(text: &str) -> String {
    format!("*** {} ***", text)
}

pub fn hello(name: &str) -> String {
    decorate(&format!("สวัสดี {}", name))
}
```

`src/main.rs`

```rust
use thai_greeter::greeting::hello;

fn main() {
    println!("{}", hello("Rust"));
}
```

**Explanation**

- **ก.** มี **2 crate ใน 1 package**: library crate (root คือ `src/lib.rs`) และ binary crate (root คือ `src/main.rs`) ทั้งคู่ชื่อ `thai_greeter` ตามชื่อ package
- **ข.** compile ไม่ผ่าน เกิด `E0603: function 'decorate' is private` เพราะ `decorate` ไม่มี `pub` จึงใช้ได้เฉพาะใน module `greeting` (และลูกหลาน) `main.rs` เป็นคนละ crate จึงเข้าถึงไม่ได้ ทำให้ผู้เขียน library เปลี่ยน/ลบ `decorate` ได้ในอนาคตโดยไม่ทำให้โค้ดของผู้ใช้พัง
- `lib.rs` ทำหน้าที่เป็น **crate root** ของ library: การเขียน `pub mod greeting;` คือการบอกว่า "โหลด `greeting.rs` มาเป็น module และเปิดให้ crate อื่นเห็น"
- `main.rs` ใช้ library เหมือนใช้ crate ภายนอก จึงต้องอ้างผ่านชื่อ crate (`thai_greeter::...`) ไม่ใช่ `crate::...`

---
## 9. PPL Perspective

ในมุมมองของ **Principles of Programming Languages (PPL)** ระบบ Module ของ Rust ช่วยจัดโครงสร้างโปรแกรมขนาดใหญ่ โดยสามารถจัดกลุ่ม Functionality ที่เกี่ยวข้อง แยกส่วนของ Code ที่มีหน้าที่แตกต่างกัน และกำหนดว่าส่วนใดของโปรแกรมสามารถเข้าถึงได้จากภายนอก

Module System ของ Rust ประกอบด้วยแนวคิดสำคัญ ได้แก่

- **Packages** — เป็นความสามารถของ Cargo ที่ใช้ Build, Test และ Share Crates
- **Crates** — เป็น Tree ของ Modules ที่สามารถสร้างเป็น Library หรือ Executable
- **Modules และ `use`** — ใช้ควบคุม Organization, Scope และ Privacy ของ Paths
- **Paths** — ใช้ระบุตำแหน่งหรือชื่อของ Item เช่น Function, Struct หรือ Module

---

### 9.1 Syntax

Rust มี Syntax หลักที่เกี่ยวข้องกับ Module System ได้แก่ `mod`, `pub`, `use` และ Path

- `mod` ใช้ประกาศ Module
- `pub` ใช้กำหนดให้ Item สามารถเข้าถึงจากภายนอกได้
- `use` ใช้นำ Path เข้ามาใน Scope เพื่อให้เรียกใช้งานได้สะดวกขึ้น
- `crate` ใช้อ้างถึง Crate Root ของ Crate ปัจจุบัน
- `super` ใช้อ้างถึง Parent Module
- `self` ใช้อ้างถึง Module ปัจจุบัน
- `::` ใช้แบ่งระดับของ Path

ตัวอย่าง:

```rust
mod food {
    pub fn order() {
        println!("Order: Pizza");
    }
}

use crate::food::order;

fn main() {
    order();
}
```

ในตัวอย่างนี้

- `mod food` สร้าง Module ชื่อ `food`
- `pub fn order()` ทำให้ Function `order()` สามารถเข้าถึงจากภายนอก Module ได้
- `use crate::food::order` นำ Path ของ Function `order()` เข้ามาใน Scope ปัจจุบัน
- `crate` หมายถึงเริ่ม Path จาก Crate Root
- หลังจากใช้ `use` แล้ว สามารถเรียก `order()` ได้โดยไม่ต้องเขียน Path เต็ม

---

### 9.2 Semantics

Package, Crate และ Module มีหน้าที่แตกต่างกันในการจัดโครงสร้างโปรแกรม Rust

```text
Package
├── Binary Crate(s)
│   └── Module Tree
│       └── Item(s)
│
└── Library Crate (optional)
    └── Module Tree
        └── Item(s)
```

#### Package

**Package** เป็นความสามารถของ Cargo ที่ใช้สำหรับ Build, Test และ Share Crates

Package ประกอบด้วยไฟล์ `Cargo.toml` ที่อธิบายวิธี Build Crates ภายใน Package

Package สามารถมี

- Binary Crates ได้หลายตัว
- Library Crate ได้ไม่เกินหนึ่งตัว
- ต้องมีอย่างน้อยหนึ่ง Crate

#### Crate

**Crate** เป็นหน่วยของโปรแกรม Rust ที่ประกอบด้วย Tree ของ Modules และสามารถสร้างเป็น

- Binary Crate — โปรแกรมที่สามารถ Execute ได้
- Library Crate — Code ที่ออกแบบมาให้โปรแกรมอื่นนำไปใช้

Crate Root คือ Source File ที่ Rust Compiler ใช้เป็นจุดเริ่มต้นในการสร้าง Root Module ของ Crate

#### Module

**Module** ใช้จัดกลุ่ม Code ภายใน Crate และช่วยควบคุม

- Organization
- Scope
- Privacy

Module สามารถมี Item ต่าง ๆ เช่น Function, Struct, Enum และ Module อื่นอยู่ภายในได้

#### Path

**Path** เป็นวิธีที่ Rust ใช้ระบุตำแหน่งของ Item ภายใน Module Tree

ตัวอย่าง:

```rust
food::order();
```

`food::order()` หมายถึง การเข้าไปที่ Module `food` แล้วเรียกใช้ Function `order()` ที่อยู่ภายใน Module นั้น

Rust รองรับ Path สองรูปแบบหลัก ได้แก่

```rust
crate::food::order();  // Absolute Path
food::order();         // Relative Path
```

- **Absolute Path** เริ่มจาก Crate Root
- **Relative Path** เริ่มจาก Module ปัจจุบัน

Relative Path ยังสามารถใช้ `self` และ `super` เพื่ออ้างอิงตำแหน่งภายใน Module Tree ได้

---

### 9.3 Type System

หัวข้อ **Packages, Crates และ Modules** ใน Chapter นี้ไม่ได้มุ่งอธิบาย Type System ของ Rust โดยตรง แต่ Module System สามารถจัดกลุ่ม Item ที่มี Type ต่าง ๆ เช่น Struct, Enum และ Function ไว้ภายใน Module ได้

ตัวอย่าง:

```rust
mod food {
    pub struct Menu {
        pub name: String,
    }

    pub fn order() {
        println!("Order: Pizza");
    }
}
```

Module `food` สามารถประกอบด้วย Item หลายประเภท เช่น

```text
food
├── Struct: Menu
└── Function: order()
```

ดังนั้นในบริบทของหัวข้อนี้ **Module ไม่ได้กำหนด Type System ของ Rust** แต่ทำหน้าที่จัดกลุ่มและกำหนดขอบเขตของ Item ต่าง ๆ ภายในโปรแกรม

Compiler จะต้องสามารถระบุได้ว่าชื่อที่อยู่ใน Scope นั้นหมายถึง Item ใด เช่น Variable, Function, Struct, Enum, Module หรือ Item ประเภทอื่น

---

### 9.4 Memory / Resource Management

หัวข้อ **Packages, Crates และ Modules** ไม่ได้เป็นกลไกสำหรับจัดการ Memory โดยตรง

หน้าที่หลักของ Module System คือการจัดการ

- Organization
- Scope
- Privacy
- Paths
- Public และ Private Interface

ตัวอย่าง:

```rust
mod food {
    pub fn order() {
        println!("Order: Pizza");
    }
}

fn main() {
    food::order();
}
```

Module `food` ทำหน้าที่จัดกลุ่ม Function `order()` และกำหนดว่าส่วนใดสามารถเข้าถึงจากภายนอกได้

ดังนั้นในบริบทของ Chapter นี้ Package, Crate และ Module เน้น **การจัดโครงสร้างและการเข้าถึง Code** มากกว่าการจัดการ Memory หรือ Resource โดยตรง

---

### 9.5 Abstraction / Other PPL Concepts

#### Abstraction

Module ช่วยให้สามารถรวม Functionality ที่เกี่ยวข้องไว้ในส่วนเดียวกัน และเปิดให้ Code ภายนอกเรียกใช้งานผ่าน Public Interface โดยไม่จำเป็นต้องรู้รายละเอียดการทำงานภายใน

ตัวอย่าง:

```rust
mod food {
    pub fn order() {
        prepare();
        println!("Order: Pizza");
    }

    fn prepare() {
        println!("Preparing...");
    }
}

fn main() {
    food::order();
}
```

ในตัวอย่างนี้

- `order()` เป็น Public Interface ที่ Code ภายนอกสามารถเรียกใช้งานได้
- `prepare()` เป็น Implementation Detail ภายใน Module

ผู้ใช้งานเพียงเรียก

```rust
food::order();
```

โดยไม่จำเป็นต้องรู้ว่า `order()` เรียก `prepare()` หรือทำงานภายในอย่างไร

#### Encapsulation

Module System ช่วย **Encapsulate Implementation Details** หรือซ่อนรายละเอียดการทำงานภายใน

```text
Module: food
├── order()      → Public
└── prepare()    → Private
```

Programmer สามารถกำหนดว่าส่วนใดเป็น Public Interface และส่วนใดเป็น Private Implementation Detail

วิธีนี้ช่วยให้สามารถแก้ไขรายละเอียดการทำงานภายในได้ โดย Code ภายนอกยังสามารถเรียก Public Interface เดิมได้

#### Scope

**Scope** คือขอบเขตที่ชื่อของ Item สามารถถูกมองเห็นและนำมาใช้งานได้

ในแต่ละ Scope อาจมีชื่อของ

- Variable
- Function
- Struct
- Enum
- Module
- Constant
- Item อื่น ๆ

Compiler ต้องสามารถระบุได้ว่าชื่อที่ถูกใช้งานในตำแหน่งหนึ่งหมายถึง Item ใด

และไม่สามารถมี Item สองตัวที่มีชื่อเดียวกันอยู่ใน Scope เดียวกันได้โดยตรง

#### Visibility and Privacy

Item ภายใน Module เป็น **Private โดย Default**

ตัวอย่าง:

```rust
mod food {
    fn order() {
        println!("Order: Pizza");
    }
}
```

Function `order()` ไม่สามารถเรียกจากภายนอก Module `food` ได้โดยตรง

หากต้องการให้ภายนอกเข้าถึง ต้องใช้ `pub`

```rust
mod food {
    pub fn order() {
        println!("Order: Pizza");
    }
}

fn main() {
    food::order();
}
```

ดังนั้น `pub` ใช้กำหนด Public Interface ของ Module

#### `use` and Scope

`use` ใช้นำ Path เข้ามาใน Scope เพื่อให้สามารถเรียก Item ได้สะดวกขึ้น

จากเดิม:

```rust
crate::food::order();
```

สามารถเขียน:

```rust
use crate::food::order;

fn main() {
    order();
}
```

การใช้ `use` ไม่ได้ย้าย Item ไปยังตำแหน่งใหม่ แต่ทำให้ Path นั้นสามารถถูกอ้างถึงด้วยชื่อที่สะดวกขึ้นภายใน Scope ปัจจุบัน

---

### 9.6 Why Rust?

เมื่อโปรแกรมมีขนาดใหญ่ การจัด Organization ของ Code จะมีความสำคัญมากขึ้น

Rust จึงมี Module System ที่ช่วย

- จัดกลุ่ม Functionality ที่เกี่ยวข้องกัน
- แยก Code ที่มีหน้าที่แตกต่างกัน
- ระบุตำแหน่งของ Code ที่ต้องการแก้ไขได้ง่ายขึ้น
- แบ่งโปรแกรมออกเป็นหลาย Modules และหลาย Files
- กำหนด Public Interface และ Private Implementation Details
- จัดการ Scope ของชื่อภายในโปรแกรม

Package สามารถมีหลาย Binary Crates และมี Library Crate ได้ไม่เกินหนึ่งตัว และเมื่อโปรแกรมมีขนาดใหญ่ขึ้น ยังสามารถแยกบางส่วนออกเป็น Crate อื่นและนำมาใช้เป็น External Dependency ได้

ดังนั้นจุดสำคัญของ Module System ใน Rust คือการช่วยจัดการ **Organization, Scope, Privacy, Paths และ Encapsulation** ของโปรแกรมอย่างเป็นระบบ

---

# 10. Rust vs. Other Languages

**Comparison Languages:** Java / C++ / Python

| Aspect | Rust | Java | C++ | Python |
|---|---|---|---|---|
| **Syntax** | ใช้ `mod` ประกาศ Module, `pub` กำหนดการเข้าถึง และ `use` นำ Path เข้ามาใน Scope | ใช้ `package` จัดกลุ่ม Class/Interface และ `import` นำ Type จาก Package อื่นมาใช้ | ใช้ `module`, `export`, `import` ใน C++20 Modules | ไฟล์ `.py` สามารถเป็น Module และใช้ `import` นำ Module อื่นมาใช้ |
| **Semantics / Behavior** | Package → Crate → Module โดย Crate เป็น Tree of Modules | Package ใช้จัดกลุ่ม Related Types และเป็น Namespace | Module ประกอบด้วย Module Units และสามารถ Export Declarations ให้ Translation Unit อื่นใช้ได้ | Package → Module โดย Module ใช้จัดกลุ่ม Function, Class และข้อมูลที่เกี่ยวข้อง |
| **Type System** | Module สามารถเก็บ Item เช่น Function, Struct และ Enum แต่ Module System ไม่ได้กำหนด Type System โดยตรง | Package จัดกลุ่ม Class และ Interface | Module สามารถประกอบด้วย Function, Class และ Type | Module สามารถเก็บ Function และ Class |
| **Memory Management** | Module System ไม่จัดการ Memory โดยตรง | Package ไม่จัดการ Memory โดยตรง | Module ไม่จัดการ Memory โดยตรง | Module/Package ไม่จัดการ Memory โดยตรง |
| **Safety** | Item เป็น Private by Default และใช้ `pub` เมื่อต้องการเปิดให้เข้าถึง | ใช้ `public`, `private`, `protected` | ใช้ `export` เปิด Declaration จาก Module และใช้ Access Specifiers ภายใน Class | ไม่มี `pub` แบบ Rust โดยทั่วไปใช้ Convention เช่น `_name` สำหรับ Non-public |

---

## Rust Example

```rust
mod food {
    pub fn order() {
        println!("Order: Pizza");
    }
}

fn main() {
    food::order();
}
```

### Structure

```text
Package
└── Crate
    └── Module: food
        └── Function: order()
```

### Explanation

- `mod food` ใช้ประกาศ Module ชื่อ `food`
- `pub fn order()` กำหนดให้ Function `order()` สามารถเข้าถึงจากภายนอก Module ได้
- `food::order()` เป็น Path ที่ใช้เรียก Function `order()` ภายใน Module `food`
- Item ภายใน Module เป็น Private by Default หากต้องการให้ภายนอกเข้าถึงต้องใช้ `pub`

---

## Java Example

### Food.java

```java
package food;

public class Food {
    public static void order() {
        System.out.println("Order: Pizza");
    }
}
```

### Main.java

```java
import food.Food;

public class Main {
    public static void main(String[] args) {
        Food.order();
    }
}
```

### Structure

```text
Package: food
└── Class: Food
    └── Method: order()
```

### Explanation

- `package food` กำหนดให้ Class `Food` อยู่ใน Package `food`
- `public class Food` ทำให้ Class สามารถเข้าถึงจากภายนอกได้
- `import food.Food` นำ Class `Food` จาก Package `food` มาใช้
- `Food.order()` เรียก Method `order()` ผ่าน Class `Food`
- Java ใช้ Access Modifiers เช่น `public`, `private`, `protected` เพื่อควบคุมการเข้าถึง

---

## C++ Example

ตัวอย่างนี้ใช้ **C++20 Modules**

### food.cppm

```cpp
export module food;

import <iostream>;

export void order() {
    std::cout << "Order: Pizza\n";
}
```

### main.cpp

```cpp
import food;

int main() {
    order();
    return 0;
}
```

### Structure

```text
Module: food
└── Exported Function: order()

main.cpp
└── import food
    └── order()
```

### Explanation

- `export module food;` ประกาศ Module ชื่อ `food`
- `export void order()` เปิด Function `order()` ให้ Code ที่ Import Module สามารถเรียกใช้งานได้
- `import food;` นำ Module `food` มาใช้
- `order()` เรียก Function ที่ถูก Export จาก Module
- C++20 Modules ใช้ `export` และ `import` เพื่อควบคุม Interface ระหว่าง Modules
- `export` ของ Module แตกต่างจาก `public`, `private`, `protected` ซึ่งใช้ควบคุมการเข้าถึง Member ภายใน Class

---

## Python Example

### food.py

```python
def order():
    print("Order: Pizza")
```

### main.py

```python
import food

food.order()
```

### Structure

```text
Module: food.py
└── Function: order()

main.py
└── import food
    └── food.order()
```

### Explanation

- `food.py` เป็น Module ชื่อ `food`
- `def order()` สร้าง Function `order()`
- `import food` นำ Module `food` มาใช้
- `food.order()` เรียก Function ผ่าน Namespace ของ Module
- Python ไม่มี `pub` แบบ Rust โดยทั่วไปใช้ Naming Convention เช่น `_name` เพื่อสื่อว่าเป็น Non-public

---

## Code Comparison Summary

| Language | Structure | Visibility | Import / Use | Call |
|---|---|---|---|---|
| **Rust** | Package → Crate → Module | Private Default / `pub` | `use` | `food::order()` |
| **Java** | Package → Class | Access Modifiers | `import` | `Food.order()` |
| **C++** | Module → Exported Declarations | `export` / Access Specifiers | `import` | `order()` |
| **Python** | Package → Module | Convention เช่น `_name` | `import` | `food.order()` |

---

## Analysis

### 1. Structure

- **Rust** → Package → Crate → Module
- **Java** → Package → Class/Interface
- **C++** → Module → Exported Declarations
- **Python** → Package → Module

**เหตุผลด้านการออกแบบ Rust:**  
Rust มีแนวคิด **Crate** เป็นหน่วยสำคัญของโปรแกรม โดย Crate เป็น Tree of Modules และ Package สามารถประกอบด้วยหนึ่งหรือหลาย Crates
เพื่อให้สามารถแบ่ง Functionality ที่เกี่ยวข้องออกเป็นส่วนต่าง ๆ และช่วยจัดโครงสร้างของโปรแกรมเมื่อโปรแกรมมีขนาดใหญ่ขึ้น

---

### 2. Scope & Visibility

- **Rust** → Item เป็น Private by Default และใช้ `pub` เพื่อเปิดการเข้าถึง
- **Java** → ใช้ `public`, `private`, `protected`
- **C++** → ใช้ `export` เพื่อเปิด Declaration จาก Module และใช้ Access Specifiers ภายใน Class
- **Python** → ไม่มี `pub` แบบ Rust และมักใช้ Convention เช่น `_name`

**เหตุผลด้านการออกแบบ Rust:**  
Rust จึงทำให้การกำหนด **Public Interface และ Private Implementation** เป็นส่วนสำคัญของ Module System
 เพื่อสนับสนุน **Encapsulation** โดยซ่อน Implementation Details และเปิดเผยเฉพาะส่วนที่ต้องการให้ Code ภายนอกใช้งาน

---

### 3. Syntax & Organization

- **Rust** → `mod`, `pub`, `use`
- **Java** → `package`, `import`
- **C++** → `module`, `export`, `import`
- **Python** → `.py`, `import`

ใน Rust แต่ละคำสั่งมีหน้าที่ที่ชัดเจน

- `mod` → ประกาศ Module
- `pub` → กำหนด Visibility
- `use` → นำ Path เข้ามาใน Scope
- Path → ระบุตำแหน่งของ Item ภายใน Module Tree

**เหตุผลด้านการออกแบบ Rust:**  
เพื่อจัดการ **Organization, Scope และ Privacy** และช่วยให้การอ้างอิง Item ภายใน Module Tree มีโครงสร้างที่ชัดเจน

---

### 4. Key Difference / Language Design

Rust ออกแบบ Module System โดยเน้น **Modularity + Scope + Privacy + Encapsulation**

Rust สามารถแบ่ง Code เป็น

```text
Package
└── Crate
    └── Module
        └── Item
```

และควบคุมว่า Item ใดสามารถเข้าถึงจากภายนอกได้ด้วย `pub`

เมื่อเปรียบเทียบกับภาษาอื่น

- **Java** เน้นการจัดกลุ่ม Code ผ่าน Package และ Class พร้อม Access Modifiers
- **C++** ใช้ Module เพื่อแบ่งและ Export Declarations และมี Access Specifiers สำหรับ Class
- **Python** ใช้ Package และ Module ที่เรียบง่ายกว่า และมักใช้ Naming Convention สำหรับ Non-public API

ดังนั้นความแตกต่างสำคัญของ Rust คือการรวม **Module Tree, Path, Scope และ Privacy** เข้ามาเป็นส่วนหนึ่งของ Module System ทำให้สามารถจัดโครงสร้าง Code และควบคุม Public/Private Interface ได้อย่างชัดเจน

---

## References

- The Rust Programming Language — Managing Growing Projects with Packages, Crates, and Modules  
  https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html

- C++ Reference — Modules (C++20)  
  https://en.cppreference.com/w/cpp/language/modules

- Java Tutorials — Creating and Using Packages  
  https://docs.oracle.com/javase/tutorial/java/package/index.html

- Python Documentation — Modules  
  https://docs.python.org/3/tutorial/modules.html
  
---
