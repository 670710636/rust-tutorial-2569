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

### Individual Contribution — Member 4

**Member 4**

รับผิดชอบส่วน **Exercises + Common Mistakes + Challenge** ได้แก่
(1) จัดทำ Common Mistakes 4 ข้อ (ลืม `pub` ที่ฟังก์ชัน, field ของ struct ยัง private, ลืมประกาศ `mod`, module ลูกไม่เห็นชื่อของ module แม่) พร้อมโค้ดที่ผิด/ถูกและอธิบายสาเหตุ
(2) ออกแบบแบบฝึกหัด 2 ข้อ (module ซ้อนกันด้วย `use ... as`, และ package ที่มี library + binary crate) พร้อมเฉลยและคำอธิบาย
(3) ออกแบบคำถามท้าทายสั้น ๆ จากแบบฝึกหัดที่ 1 สำหรับให้ผู้ฟังตอบสดในชั้นเรียน
(4) ทดสอบ compile/run โค้ดในส่วนของตนเองทั้งหมด และ review Pull Request ของสมาชิกคนอื่น
