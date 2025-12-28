---
title: "Bài 9: Document Object Model (DOM) trong JS"
date: 2025-12-26T16:00:00+07:00
draft: false
tags: ["JavaScript", "DOM"]
cover:
    image: "https://images.unsplash.com/photo-1507721999472-8ed4421c4af2"
    alt: "Website DOM"
---

## 1. Bản chất và Cấu trúc cây DOM
**DOM (Document Object Model)** không phải là file HTML bạn viết, và nó cũng không phải là những gì bạn nhìn thấy trên trình duyệt (Render Tree).
DOM là một **Giao diện lập trình ứng dụng (API)**. Khi trình duyệt tải một trang web, nó phân tích cú pháp HTML và tạo ra một mô hình trong bộ nhớ dưới dạng **Cây cấu trúc (Tree Structure)**.

[Image of DOM tree structure diagram root-parent-child]

### Các thành phần của cây DOM:
Mọi thứ trong DOM đều được gọi là **Node (Nút)**. Có 3 loại Node chính mà lập trình viên cần quan tâm:
1.  **Element Node:** Đại diện cho thẻ HTML (ví dụ: `<div>`, `<body>`, `<h1>`). Đây là khung xương của trang web.
2.  **Text Node:** Chứa nội dung văn bản bên trong thẻ. Lưu ý: Ngay cả khoảng trắng hay xuống dòng cũng có thể được tính là một Text Node.
3.  **Attribute Node:** Các thuộc tính của thẻ (như `class`, `id`, `href`).

Mối quan hệ gia phả:
* **Root:** `<html>` là gốc.
* **Parent/Child:** `<body>` là con của `<html>`. `<div>` nằm trong `<body>` là con của `<body>`.
* **Sibling:** Các thẻ nằm cùng cấp gọi là anh em.

## 2. Các phương thức Truy xuất (Selecting Elements)
Để thao tác, trước tiên bạn phải "tóm" được phần tử đó.

### a. Nhóm phương thức cũ (Legacy)
Trả về một **HTMLCollection** (Danh sách động - Live). Nếu DOM thay đổi, danh sách này tự cập nhật theo.
* `document.getElementById('id')`: Nhanh nhất, lấy theo ID duy nhất.
* `document.getElementsByClassName('class')`: Lấy danh sách các thẻ có cùng class.
* `document.getElementsByTagName('div')`: Lấy tất cả thẻ `div`.

### b. Nhóm phương thức hiện đại (Modern - Khuyên dùng)
Linh hoạt hơn vì sử dụng CSS Selector, nhưng trả về **NodeList** (Danh sách tĩnh - Static).
* `document.querySelector('.class #id')`: Trả về phần tử **đầu tiên** tìm thấy khớp với CSS Selector.
* `document.querySelectorAll('div.box')`: Trả về **tất cả** phần tử khớp.

## 3. Thao tác và Chỉnh sửa (Manipulation)
Sau khi truy xuất, ta có thể phẫu thuật trang web.

### a. Thay đổi nội dung
Cần phân biệt rõ 3 thuộc tính:
* `innerHTML`: Lấy/Gán cả mã HTML bên trong. *Cảnh báo: Dễ dính lỗ hổng bảo mật XSS nếu gán dữ liệu người dùng trực tiếp.*
* `innerText`: Chỉ lấy nội dung văn bản mà người dùng **nhìn thấy** (bỏ qua thẻ ẩn `display: none`).
* `textContent`: Lấy toàn bộ văn bản thô, bao gồm cả các thẻ ẩn và khoảng trắng. (Hiệu năng cao nhất).

### b. Thay đổi kiểu dáng (Style & Class)
* **Trực tiếp (Inline Style):** `element.style.backgroundColor = 'red'`. Nhược điểm là ghi đè style inline, khó quản lý.
* **Thông qua Class (Best Practice):** Dùng `element.classList`.
    * `.add('dark-mode')`: Thêm class.
    * `.remove('active')`: Xóa class.
    * `.toggle('hidden')`: Nếu có thì xóa, chưa có thì thêm.

### c. Tạo và Thêm phần tử mới
```javascript
const newDiv = document.createElement('div'); // 1. Tạo thẻ div
newDiv.textContent = "Xin chào";              // 2. Thêm nội dung
document.body.appendChild(newDiv);            // 3. Gắn vào DOM thật
