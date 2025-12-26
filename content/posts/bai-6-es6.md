---
title: "Bài 6: JavaScript ES6 - Scoping với Let và Const"
date: 2025-12-26T13:00:00+07:00
draft: false
tags: ["JavaScript", "ES6"]
cover:
    image: "https://images.unsplash.com/photo-1627398242454-45a1465c2479"
    alt: "JS Code"
---

## 1. Vấn đề của 'var'
Trong JS cũ, `var` có phạm vi là function scope và hỗ trợ hoisting (đưa khai báo lên đầu). Điều này dễ gây lỗi logic khi biến bị ghi đè không mong muốn.

## 2. Block Scope với 'let' và 'const'
ES6 giới thiệu `let` và `const` với phạm vi khối (Block scope - trong cặp ngoặc `{}`).

* **let:** Dùng cho biến có thể thay đổi giá trị.
* **const:** Dùng cho hằng số (không thể gán lại giá trị).

## 3. Ví dụ minh họa
```javascript
// Ví dụ về Block Scope
if (true) {
    var x = 10;
    let y = 20;
}

console.log(x); // 10 (Vẫn truy cập được -> Nguy hiểm)
console.log(y); // Error: y is not defined (An toàn)
