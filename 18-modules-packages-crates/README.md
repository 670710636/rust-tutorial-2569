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

### Runable Code Example

ตัวอย่างต่อไปนี้แสดงการทำงานของ Modules, Visibility, `pub`, `use`,
Package, Crate และการจัดโครงสร้างโปรเจกต์ในภาษา Rust

ตัวอย่างทั้งหมดใช้โปรเจกต์ `modules_demo` และสามารถนำไปทดลองรัน
เพื่อดูผลลัพธ์ได้จริง

## Example 1 — Module, Visibility และ `pub`

ตัวอย่างนี้แสดงวิธีสร้าง Module และกำหนดว่า Function ใดสามารถ
ถูกเรียกใช้งานจากภายนอก Module ได้

 Code ตัวอย่าง

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

**Comparison Language:** Java / C / Python

| Aspect | Rust | Java | C | Python |
|---|---|---|---|---|
| **Syntax** | ใช้ `mod` สร้าง Module, `pub` กำหนด Visibility และ `use` นำ Path เข้ามาใน Scope | ใช้ `package` จัดกลุ่ม Class/Interface และ `import` นำ Class มาใช้ | ใช้ Source File `.c`, Header File `.h` และ `#include` | ไฟล์ `.py` สามารถเป็น Module และใช้ `import` นำ Module มาใช้ |
| **Semantics / Behavior** | Package จัดการ Crates, Crate เป็น Tree ของ Modules และ Module ใช้ควบคุม Organization, Scope และ Privacy | Package ใช้จัดกลุ่ม Class/Interface และเป็น Namespace | ใช้ Source File, Header File และ Linkage ในการแบ่งโปรแกรม | Module ใช้จัดกลุ่ม Code และ Package สามารถรวมหลาย Modules |
| **Type System** | Module สามารถประกอบด้วย Item เช่น Function, Struct และ Enum โดย Module System ไม่ได้เป็นตัวกำหนด Type System โดยตรง | Class/Interface เป็นส่วนสำคัญของโครงสร้าง Type ของ Java | Type ถูกประกาศใน Source/Header Files | Function และ Class สามารถอยู่ภายใน Module |
| **Memory Management** | Package, Crate และ Module ไม่ได้จัดการ Memory โดยตรง แต่ใช้จัด Organization, Scope และ Privacy | Package ไม่ได้เป็นกลไกจัดการ Memory โดยตรง | Source/Header Files ไม่ได้เป็นกลไกจัดการ Memory โดยตรง | Module/Package ไม่ได้เป็นกลไกจัดการ Memory โดยตรง |
| **Safety / Visibility** | Item เป็น Private โดย Default และใช้ `pub` เมื่อต้องการเปิดให้ภายนอกเข้าถึง | ใช้ Access Modifiers เช่น `public`, `private`, `protected` | ใช้ Scope และ Linkage เช่น `static`, `extern` | ไม่มี `pub` แบบ Rust และมักใช้ Naming Convention เช่น `_name` |

> **หมายเหตุ:** ตารางนี้เน้นเปรียบเทียบในบริบทของ **Modules, Packages & Crates** เพื่อให้สอดคล้องกับหัวข้อของ Tutorial

---

## Code Examples

เพื่อให้เห็นความแตกต่างชัดเจน ตัวอย่างทุกภาษาจะทำงานเหมือนกัน คือสร้างส่วน `food` ที่มี `order()` สำหรับแสดงข้อความ:

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
- `#include` ใช้นำเนื้อหาจาก Header มาใช้
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

จากตัวอย่างจะเห็นว่าทั้ง 4 ภาษาใช้แนวคิด **Modularity** เพื่อแบ่งโปรแกรมออกเป็นส่วนย่อย แต่ใช้กลไกที่แตกต่างกัน

```text
Rust                    Java
Package                 Package
└── Crate               └── Class: Food
    └── Module: food        └── Method: order()
        └── Function:
            order()


C                       Python
Header + Source         Module: food.py
└── Function: order()   └── Function: order()
```

**Rust** ใช้ Package, Crate และ Module เป็นส่วนสำคัญในการจัด Organization ของโปรแกรม โดย Module และ `use` ช่วยควบคุม Organization, Scope และ Privacy ส่วน Path ใช้ระบุตำแหน่งของ Item ภายใน Module Tree

**Java** ใช้ Package ในการจัดกลุ่ม Class และ Interface และใช้ Access Modifier เพื่อควบคุมการเข้าถึง

**C** ไม่มี Module System แบบ Rust โดยตรง แต่สามารถแบ่ง Code ออกเป็น Source File และ Header File และใช้ Scope และ Linkage ในการควบคุมการมองเห็นของชื่อ

**Python** ใช้ไฟล์ `.py` เป็น Module และใช้ `import` เพื่อนำ Module มาใช้

ในมุมมองของ PPL จุดสำคัญของ Rust Module System คือ **Modularity, Scope, Namespace, Visibility, Privacy และ Encapsulation** โดย Programmer สามารถกำหนด Public Interface และซ่อน Private Implementation Details ได้อย่างชัดเจน

เมื่อโปรแกรมมีขนาดใหญ่ขึ้น แนวทางนี้ช่วยจัดกลุ่ม Functionality ที่เกี่ยวข้อง แยก Code ที่มีหน้าที่แตกต่างกัน และทำให้สามารถระบุตำแหน่งของ Code ที่ต้องการแก้ไขได้ง่ายขึ้น

---

## References

- The Rust Programming Language — Packages, Crates, and Modules  
  https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html
