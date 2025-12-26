---
title: "Bài 9: Document Object Model (DOM) trong JS"
date: 2025-12-26T16:00:00+07:00
draft: false
tags: ["JavaScript", "DOM"]
cover:
    image: "https://images.unsplash.com/photo-1507721999472-8ed4421c4af2"
    alt: "Website DOM"
---

## 1. Cấu trúc cây DOM
Khi trình duyệt tải một trang web, nó chuyển đổi HTML thành một cây đối tượng gọi là DOM. Mỗi thẻ HTML là một Node (nút). JavaScript có thể truy cập vào DOM để thay đổi nội dung, kiểu dáng (CSS) hoặc cấu trúc trang web động.

## 2. Các thao tác cơ bản
* **Truy xuất phần tử:** `document.getElementById()`, `document.querySelector()`.
* **Thay đổi nội dung:** `.innerHTML`, `.textContent`.
* **Thay đổi Style:** `.style.property`.

## 3. Ví dụ đổi màu nền
```javascript
// Lấy nút bấm
const btn = document.querySelector('#btn-change');

// Lắng nghe sự kiện click
btn.addEventListener('click', () => {
    // Thay đổi CSS trực tiếp
    document.body.style.backgroundColor = '#333';
    document.body.style.color = '#fff';
    alert("Đã chuyển sang giao diện tối!");
});
