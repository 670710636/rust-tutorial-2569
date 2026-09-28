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

## 9. PPL Perspective

ในมุมมองของ **Principles of Programming Languages (PPL)** แนวคิด **Packages, Crates และ Modules** ของ Rust ช่วยจัดโครงสร้างโปรแกรม กำหนดขอบเขตของชื่อ (Scope / Namespace) และควบคุมการเข้าถึงส่วนต่าง ๆ ของโปรแกรมอย่างชัดเจน

### 9.1 Syntax

Rust มี Syntax ที่ใช้ในการสร้าง Module และกำหนดการเข้าถึง Item ต่าง ๆ ภายใน Module ได้แก่

- `mod` ใช้ประกาศ Module
- `pub` ใช้กำหนดให้ Item สามารถเข้าถึงจากภายนอกได้
- `use` ใช้นำชื่อหรือ Path เข้ามาใน Scope ปัจจุบัน
- `crate` ใช้อ้างถึง Crate ปัจจุบัน
- `super` ใช้อ้างถึง Parent Module
- `self` ใช้อ้างถึง Module ปัจจุบัน
-  `::` ใช้แบ่งระดับของ Path เพื่อระบุตำแหน่งของ Module หรือ Item

ตัวอย่าง:

```rust
mod food {
    pub fn order() {
        println!("Order food");
    }
}

use crate::food::order;

fn main() {
    order();
}
```

ในตัวอย่างนี้

- `mod food` สร้าง Module ชื่อ `food`
- `pub fn order()` กำหนดให้ Function `order()` สามารถเข้าถึงจากภายนอก Module ได้
- `use crate::food::order` นำ Function `order` เข้ามาใน Scope ปัจจุบัน
    - use → นำชื่อเข้ามาใน Scope ปัจจุบัน
    - crate → เริ่มค้นหาจาก Crate ปัจจุบัน
    - food → Module ชื่อ food
    - order → Item ที่อยู่ใน food เช่น Function order()
- `order()` จึงสามารถถูกเรียกใช้ใน `main()` ได้โดยไม่ต้องเขียน Path เต็ม

---

### 9.2 Semantics

ใน Rust **Package, Crate และ Module** มีความหมายและหน้าที่แตกต่างกัน

- **Package** คือหน่วยของ Cargo Project ใช้สำหรับจัดการโปรเจกต์และรวบรวม Crate ที่เกี่ยวข้อง
- **Crate** คือหน่วยของโปรแกรมที่ Rust Compiler นำไป Compile โดยสามารถเป็น **Binary Crate** หรือ **Library Crate**
- **Module** ใช้แบ่งและจัดกลุ่ม Code ภายใน Crate รวมถึงช่วยสร้าง Namespace
- **Item** คือสิ่งที่สามารถอยู่ภายใน Module เช่น Function, Struct, Enum หรือ Constant

ตัวอย่าง Path:

```text
crate::food::order
```

สามารถอธิบายได้ว่า

1. `crate` เริ่มต้นจาก Crate ปัจจุบัน
2. `food` คือ Module ภายใน Crate
3. `order` คือ Item ที่อยู่ภายใน Module `food`

ดังนั้น Package และ Crate ช่วยกำหนดโครงสร้างระดับ Project และ Compilation ส่วน Module ช่วยแบ่ง Code ภายใน Crate ออกเป็นหมวดหมู่และกำหนดวิธีเข้าถึง Item ต่าง ๆ

---

### 9.3 Type System

Rust เป็นภาษาแบบ **Static Typing** ซึ่ง Type จะถูกตรวจสอบในช่วง **Compile Time** ก่อนที่โปรแกรมจะทำงาน

สำหรับ **Modules, Packages และ Crates** นั้น Module System ไม่ได้เป็น Type System โดยตรง แต่ทำงานร่วมกับ Type System โดยช่วยกำหนด Scope และ Visibility ของ Type และ Item ต่าง ๆ

ตัวอย่าง:

```rust
mod user {
    pub struct User {
        pub name: String,
    }
}

fn main() {
    let u = user::User {
        name: String::from("Alice"),
    };

    println!("{}", u.name);
}
```

จากตัวอย่าง

- `User` เป็น `struct` ที่สร้าง Type ขึ้นมา
- `name` มี Type เป็น `String`
- `pub struct User` ทำให้ Type `User` สามารถเข้าถึงจากภายนอก Module `user`
- `pub name` ทำให้ Field `name` สามารถเข้าถึงจากภายนอกได้
- Compiler ตรวจสอบทั้งความถูกต้องของ Type และการเข้าถึง Item

ดังนั้น **Type System** ทำหน้าที่ตรวจสอบความถูกต้องของ Type ส่วน **Module System และ Visibility** ช่วยควบคุมว่าส่วนใดของโปรแกรมสามารถมองเห็นและใช้งาน Type หรือ Item นั้นได้

---

### 9.4 Memory / Resource Management

**Packages, Crates และ Modules ไม่ได้ทำหน้าที่จัดการ Memory โดยตรง** แต่ Code ที่อยู่ภายใน Module ยังคงทำงานภายใต้กฎการจัดการ Memory ของ Rust

Rust ใช้แนวคิดสำคัญในการจัดการ Memory ได้แก่

- **Ownership** — กำหนดว่า Value ใดมีตัวแปรใดเป็นเจ้าของ
- **Borrowing** — อนุญาตให้ยืม Value ไปใช้งานโดยไม่จำเป็นต้องย้าย Ownership
- **References** — ใช้อ้างอิง Value เช่น `&String`
- เมื่อ Value หมด Scope ทรัพยากรที่ Value นั้นเป็นเจ้าของจะถูกปล่อยตามกฎของ Rust

ตัวอย่าง:

```rust
mod message {
    pub fn show(text: &String) {
        println!("{}", text);
    }
}

fn main() {
    let text = String::from("Hello");

    message::show(&text);

    println!("{}", text);
}
```

Function `show()` อยู่ภายใน Module `message` และรับ Parameter เป็น `&String` ซึ่งเป็น Reference

`message::show(&text)` จึงเป็นการ Borrow ข้อมูลแทนการย้าย Ownership เข้าไปใน Function

หลังจาก Function `show()` ทำงานเสร็จ Ownership ของ `text` ยังคงอยู่ใน `main()` ทำให้สามารถใช้ `text` ต่อได้

ดังนั้นการแบ่ง Code เป็น Module ไม่ได้เปลี่ยนกฎของ Ownership และ Borrowing แต่ช่วยจัดโครงสร้างของ Code ที่ใช้กฎเหล่านี้ให้ชัดเจนขึ้น

---

### 9.5 Abstraction / Other PPL Concepts

Modules, Packages และ Crates เกี่ยวข้องกับแนวคิดทาง PPL หลายด้าน ได้แก่ **Abstraction, Scope, Visibility และ Namespace**

#### Abstraction

Module ช่วยสร้าง **Abstraction** โดยสามารถซ่อนรายละเอียดการทำงานภายใน และเปิดเผยเฉพาะ Item ที่ต้องการให้ส่วนอื่นของโปรแกรมใช้งานผ่าน `pub`

ตัวอย่าง:

```rust
mod calculator {
    fn calculate(a: i32, b: i32) -> i32 {
        a + b
    }

    pub fn add(a: i32, b: i32) -> i32 {
        calculate(a, b)
    }
}
```

`calculate()` เป็นรายละเอียดภายในของ Module และไม่ได้ประกาศเป็น `pub` จึงไม่สามารถเรียกใช้โดยตรงจากภายนอกได้

ส่วน `add()` เป็น Public Interface ที่ Code ภายนอกสามารถเรียกใช้งานได้

แนวคิดนี้ทำให้ผู้ใช้งาน Module สนใจเฉพาะสิ่งที่ Module เปิดให้ใช้งาน โดยไม่จำเป็นต้องรู้รายละเอียด Implementation ภายในทั้งหมด

#### Scope

Rust ใช้ **Lexical Scope หรือ Static Scope** ซึ่งขอบเขตของชื่อสามารถพิจารณาได้จากโครงสร้างของ Source Code

แต่ละ Module สามารถมี Scope ของตัวเอง และสามารถใช้ `use` เพื่อนำชื่อจาก Path อื่นเข้ามาใน Scope ปัจจุบัน

ตัวอย่าง:

```rust
use crate::food::order;
```

ทำให้ชื่อ `order` สามารถถูกเรียกใช้ใน Scope ปัจจุบันได้โดยไม่ต้องเขียน `crate::food::order` ทุกครั้ง

#### Visibility

Item ภายใน Module เป็น **Private โดย Default**

หากต้องการให้ Code ภายนอกสามารถเข้าถึง Item ได้ จะต้องกำหนด Visibility เช่น `pub`

ตัวอย่าง:

```rust
pub fn order() {
    println!("Order food");
}
```

`pub` ทำให้ Function `order()` สามารถเข้าถึงได้จากภายนอก Module ตามกฎ Visibility ของ Rust

แนวคิดนี้ช่วยให้ Programmer ควบคุมได้ว่าส่วนใดเป็น Implementation ภายใน และส่วนใดเป็น Public Interface

#### Namespace

Module ทำหน้าที่เป็น **Namespace** ช่วยจัดกลุ่มชื่อและลดปัญหาการใช้ชื่อซ้ำกัน

ตัวอย่าง:

```text
crate::customer::create
crate::product::create
```

ทั้ง Module `customer` และ `product` สามารถมี Function ชื่อ `create` เหมือนกันได้ เพราะ Function ทั้งสองอยู่ภายใต้ Namespace ที่แตกต่างกัน

ดังนั้น Module System ของ Rust จึงสนับสนุนแนวคิดทาง PPL หลายด้าน ทั้ง **Abstraction, Scope, Visibility และ Namespace**

---

### 9.6 Why Rust?

Rust ใช้ **Package, Crate และ Module System** เพื่อช่วยให้โปรแกรมมีโครงสร้างที่ชัดเจน สามารถแบ่ง Code ออกเป็นส่วนย่อย และควบคุมการเข้าถึง Item ต่าง ๆ ได้

แนวคิดเหล่านี้มีประโยชน์ในด้านต่าง ๆ ดังนี้

- **Safety** — Item ภายใน Module เป็น Private โดย Default และ Programmer ต้องระบุ `pub` เมื่อต้องการเปิดให้ส่วนอื่นเข้าถึง
- **Reliability** — การแบ่ง Code เป็น Module ช่วยแยกหน้าที่ของแต่ละส่วน และลดการเข้าถึง Implementation ภายในโดยไม่จำเป็น
- **Maintainability** — Package, Crate และ Module ช่วยจัดโปรแกรมขนาดใหญ่ให้เป็นส่วนย่อย ทำให้ Code อ่าน แก้ไข และดูแลได้ง่ายขึ้น
- **Compile-time Checking** — Compiler สามารถตรวจสอบ Path, Visibility และการเข้าถึง Item ก่อนที่โปรแกรมจะทำงาน

ดังนั้น **Package, Crate และ Module System** ของ Rust ไม่ได้มีหน้าที่เพียงจัดไฟล์หรือแบ่ง Code เท่านั้น แต่ยังช่วยสร้าง **Abstraction, Scope, Namespace และ Visibility** ที่ชัดเจน และช่วยให้ Compiler สามารถตรวจพบข้อผิดพลาดหลายอย่างได้ตั้งแต่ Compile Time

---

## 10. Rust vs. Other Languages

**Comparison Language:** Java / C / Python

| Aspect | Rust | Java | C | Python |
|---|---|---|---|---|
| **Syntax** | ใช้ `mod` เพื่อประกาศ Module, `use` เพื่อนำชื่อจาก Module อื่นเข้ามาใช้ใน Scope และ `pub` เพื่อกำหนดให้ Item สามารถเข้าถึงจากภายนอกได้ ส่วน Package และ Crate ถูกจัดการผ่านโครงสร้างของ Cargo Project | ใช้ `package` เพื่อระบุว่า Class หรือ Interface อยู่ใน Package ใด และใช้ `import` เพื่อนำ Class หรือ Type จาก Package อื่นมาใช้งาน การเข้าถึงควบคุมด้วย `public`, `private`, `protected` เป็นต้น | แบ่งโปรแกรมออกเป็น Source File `.c` และ Header File `.h` ใช้ `#include` เพื่อนำ Declaration จาก Header มาใช้ และใช้ `static` / `extern` เพื่อควบคุม Scope และ Linkage | ไฟล์ `.py` แต่ละไฟล์สามารถเป็น Module และสามารถรวมหลาย Module เป็น Package ใช้ `import` หรือ `from ... import ...` เพื่อนำ Module หรือชื่อมาใช้งาน |
| **Semantics / Behavior** | Package เป็นหน่วยที่ Cargo ใช้จัดการโปรเจกต์, Crate เป็นหน่วย Compilation และ Module ใช้จัดโครงสร้างและแบ่ง Namespace ภายใน Crate | Package ใช้จัดกลุ่ม Class และ Interface ที่เกี่ยวข้อง และทำหน้าที่เป็น Namespace ของโปรแกรม | ไม่มีระบบ Package หรือ Module แบบ Rust โดยตรง การแบ่งโค้ดอาศัย Source File, Header File และ Linker | Module ใช้จัดกลุ่มโค้ดในแต่ละไฟล์ ส่วน Package ใช้รวม Module ที่เกี่ยวข้องเข้าด้วยกัน |
| **Type System** | เป็น Static และ Strong Typing โดย Compiler ตรวจสอบ Type ก่อนโปรแกรมทำงาน | เป็น Static และ Strong Typing โดยตรวจสอบ Type ขณะ Compile | เป็น Static Typing โดย Type ของตัวแปรถูกกำหนดและตรวจสอบตอน Compile | เป็น Dynamic Typing โดย Type ถูกกำหนดและตรวจสอบขณะ Runtime |
| **Memory Management** | ใช้ Ownership และ Borrowing จัดการ Memory โดยไม่ต้องใช้ Garbage Collector | ใช้ Garbage Collector จัดการ Memory อัตโนมัติ | Programmer จัดการ Memory เอง เช่น `malloc()` และ `free()` | ใช้ Automatic Memory Management และ Garbage Collection |
| **Safety** | Item ภายใน Module เป็น Private โดย Default และต้องใช้ `pub` เมื่อต้องการเปิดให้ภายนอกเข้าถึง นอกจากนี้ Ownership และ Borrowing ยังช่วยเพิ่ม Memory Safety | ใช้ Access Modifiers เช่น `public`, `private`, `protected` เพื่อควบคุมการเข้าถึง และ JVM ช่วยจัดการ Memory | Programmer ต้องรับผิดชอบการจัดการ Memory และการเข้าถึงข้อมูลเป็นหลัก จึงมีโอกาสเกิด Memory Error ได้มากกว่า | มี Automatic Memory Management และ Exception Handling ช่วยลดข้อผิดพลาดบางประเภท แต่ไม่มี Ownership System แบบ Rust |

---

### Rust Example

## Code Examples

เพื่อให้เห็นความแตกต่างชัดเจน ตัวอย่างทุกภาษาจะทำงานเหมือนกัน คือสร้างส่วน `food`
ที่มี `order()` สำหรับแสดงข้อความ:

```text
Order: Pizza
```

---

### Rust Example

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

**โครงสร้าง**

```text
Package
└── Crate
    └── Module: food
        └── Function: order()
```

- `mod food` สร้าง Module
- `pub` ทำให้ `order()` สามารถเรียกจากภายนอก Module ได้
- `food::order()` เรียก Function ผ่าน Module Path

---

### Java Example

**Food.java**

```java
package food;

public class Food {
    public static void order() {
        System.out.println("Order: Pizza");
    }
}
```

**Main.java**

```java
import food.Food;

public class Main {
    public static void main(String[] args) {
        Food.order();
    }
}
```

**โครงสร้าง**

```text
Package: food
└── Class: Food
    └── Method: order()
```

- `package food` กำหนด Package
- `public` กำหนดการเข้าถึง
- `import food.Food` นำ Class มาใช้
- `Food.order()` เรียก Method ผ่าน Class

---

### C Example

**food.h**

```c
#ifndef FOOD_H
#define FOOD_H

void order(void);

#endif
```

**food.c**

```c
#include <stdio.h>
#include "food.h"

void order(void) {
    printf("Order: Pizza\n");
}
```

**main.c**

```c
#include "food.h"

int main(void) {
    order();
    return 0;
}
```

**โครงสร้าง**

```text
food.h
└── Function Declaration

food.c
└── Function Definition

main.c
└── Function Call
```

- `.h` ใช้ประกาศ Interface
- `.c` ใช้เก็บ Implementation
- `#include` นำเนื้อหาจาก Header มาใช้ใน Translation Unit
- ไม่มี `mod` และ `pub` แบบ Rust

---

### Python Example

**food.py**

```python
def order():
    print("Order: Pizza")
```

**main.py**

```python
import food

food.order()
```

**โครงสร้าง**

```text
Module: food.py
└── Function: order()

main.py
└── import food
    └── food.order()
```

- `food.py` เป็น Module
- `import food` นำ Module มาใช้
- `food.order()` เรียก Function ผ่าน Namespace ของ Module
- ไม่มี `pub` แบบ Rust

---

## Code Comparison Summary

| Language | การแบ่งโปรแกรม | การควบคุมการเข้าถึง | การนำมาใช้ | การเรียก |
|---|---|---|---|---|
| **Rust** | `mod food` | `pub` | `use` (เมื่อจำเป็น) | `food::order()` |
| **Java** | `package food` + `class Food` | `public`, `private`, `protected` | `import food.Food` | `Food.order()` |
| **C** | `food.h` + `food.c` | Scope / Linkage เช่น `static`, `extern` | `#include "food.h"` | `order()` |
| **Python** | `food.py` | Convention เช่น `_name` | `import food` | `food.order()` |

---

## Analysis

จากตัวอย่างจะเห็นว่าทั้ง 4 ภาษาใช้แนวคิด **Modularity** เพื่อแบ่งโปรแกรมออกเป็นส่วนย่อยเหมือนกัน แต่ใช้กลไกต่างกัน

```text
Rust                    Java
Package                 Package
└── Crate               └── Class
    └── Module              └── Method
        └── Function


C                       Python
Header + Source         Package
└── Function            └── Module
                            └── Function / Class
```

**Rust** มี Package, Crate และ Module System เป็นโครงสร้างที่รองรับโดยภาษา และใช้ `pub` ควบคุม Visibility โดย Compiler สามารถตรวจสอบ Scope และการเข้าถึงได้ตั้งแต่ Compile Time

**Java** เน้น Package และ Class/Interface และใช้ Access Modifier เช่น `public`, `private` และ `protected` เพื่อควบคุมการเข้าถึง

**C** ไม่มี Module System แบบ Rust โดยตรง แต่ใช้ Source File, Header File, Scope และ Linkage ในการแบ่ง Interface และ Implementation

**Python** ใช้ไฟล์ `.py` เป็น Module และ `import` เพื่อนำ Module มาใช้ แต่ไม่มี Visibility Control ที่บังคับแบบ `pub` ของ Rust โดยมักใช้ Convention เช่น `_name`

**ในมุมมอง PPL** จุดเด่นของ Rust คือการจัดการ **Modularity, Namespace, Scope, Visibility** และ **Information Hiding** อย่างเป็นระบบ โดย Package, Crate และ Module ช่วยแบ่งโครงสร้างของโปรแกรม ส่วน pub และ Module Path ช่วยกำหนดการเข้าถึงและขอบเขตของชื่อ ซึ่ง Compiler สามารถตรวจสอบได้ตั้งแต่ Compile Time
