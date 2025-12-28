---
title: "Bài 6: JavaScript ES6 - Scoping với Let và Const"
date: 2025-12-26T13:00:00+07:00
draft: false
tags: ["JavaScript", "ES6"]
cover:
    image: "https://images.unsplash.com/photo-1627398242454-45a1465c2479"
    alt: "JS Code"
---

## 1. Vấn đề của 'var' (Legacy JavaScript)
Trước khi ES6 (ECMAScript 2015) ra đời, `var` là cách duy nhất để khai báo biến. Tuy nhiên, nó tồn tại hai vấn đề thiết kế lớn gây khó khăn cho việc quản lý mã nguồn lớn:

[Image of JavaScript variable scope diagram var vs let]

### a. Function Scope (Phạm vi hàm)
Biến khai báo bằng `var` có phạm vi hoạt động trong toàn bộ hàm (function) chứa nó. Nó **không tôn trọng** các khối lệnh con như `if`, `for`, `while`.
* Điều này dẫn đến việc biến bị "rò rỉ" (leak) ra ngoài vòng lặp hoặc câu lệnh điều kiện, gây xung đột tên biến hoặc ghi đè giá trị không mong muốn.

### b. Hoisting (Cơ chế đẩy lên trên)
Khi code chạy, trình biên dịch JS sẽ "nhấc" phần khai báo của `var` lên đầu phạm vi hàm.
* Biến `var` được khởi tạo giá trị mặc định là `undefined` ngay lập tức.
* Do đó, bạn có thể gọi biến trước khi khai báo mà không bị lỗi (dù giá trị là undefined), điều này đi ngược lại logic lập trình thông thường.

## 2. Block Scope với 'let' và 'const' (ES6 Modern)
ES6 giới thiệu khái niệm **Block Scope** (Phạm vi khối), được giới hạn bởi cặp ngoặc nhọn `{}`. Bất kỳ thứ gì nằm trong `{}` (của `if`, `loop`, hay đơn giản là một khối `{}` độc lập) đều là một thế giới riêng.

### a. Từ khóa 'let'
* Dùng để khai báo biến có thể thay đổi giá trị (mutable).
* Chỉ tồn tại trong block `{}` nơi nó được khai báo.
* Không thể khai báo lại (Re-declaration) trong cùng một phạm vi (tránh lỗi trùng tên vô ý).

### b. Từ khóa 'const'
* Dùng để khai báo hằng số. Bắt buộc phải gán giá trị ngay khi khai báo.
* **Lưu ý quan trọng về tính "Bất biến":**
    * Với kiểu nguyên thủy (number, string, boolean): Bạn không thể gán lại giá trị mới (`=`).
    * Với kiểu tham chiếu (Object, Array): Bạn không thể gán biến sang object khác, nhưng **có thể thay đổi thuộc tính bên trong** object đó.

## 3. Temporal Dead Zone (TDZ) - Vùng chết tạm thời
Đây là điểm khác biệt tinh tế nhất về mặt lý thuyết giữa `var` và `let/const`.
* Cả `let` và `const` thực tế **vẫn bị Hoisting** (được engine biết đến khi bắt đầu block).
* Tuy nhiên, chúng **không được khởi tạo giá trị** (khác với `var` được gán `undefined`).
* Khoảng thời gian từ khi block bắt đầu cho đến dòng code khai báo biến gọi là **TDZ**. Nếu truy cập biến trong vùng này, JS sẽ ném lỗi `ReferenceError` thay vì trả về `undefined`.

[Image of JavaScript Temporal Dead Zone diagram]

## 4. Ví dụ minh họa và So sánh

### Code ví dụ về Block Scope & TDZ
```javascript
// 1. Vấn đề của var
if (true) {
    var x = 10;
}
console.log(x); // Output: 10 (Nguy hiểm: x bị rò rỉ ra khỏi block if)

// 2. Sự an toàn của let
if (true) {
    let y = 20;
    console.log(y); // Output: 20 (Truy cập tốt trong block)
}
// console.log(y); // Error: y is not defined (An toàn: y đã bị hủy sau khi block kết thúc)

// 3. Temporal Dead Zone (TDZ)
console.log(a); // Output: undefined (Do var hoisting)
var a = 5;

// console.log(b); // Error: Cannot access 'b' before initialization (TDZ bảo vệ)
let b = 10;
