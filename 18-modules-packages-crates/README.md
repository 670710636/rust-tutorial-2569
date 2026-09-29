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
