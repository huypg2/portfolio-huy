---
title: "Bài 7 (Nâng cao): Arrow Function - Từ Cú pháp đến Bản chất"
date: 2025-12-26T14:00:00+07:00
draft: false
tags: ["JavaScript", "ES6", "Deep Dive"]
cover:
    image: "https://images.unsplash.com/photo-1587620962725-abab7fe55159"
    alt: "Coding Deep Dive"
---

## 1. Cú pháp: Những điểm "tinh tế" dễ sai
Arrow Function không chỉ giúp code ngắn hơn, mà còn có những quy tắc cú pháp riêng cần lưu ý để tránh lỗi logic (bug).

### a. Trả về Object (Returning Object Literals)
Đây là lỗi phổ biến nhất. Vì dấu ngoặc nhọn `{}` được dùng để định nghĩa khối lệnh (block code), nên nếu bạn muốn trả về một Object ngay lập tức, bạn phải bao quanh nó bằng **ngoặc tròn `()`**.

```javascript
// SAI: JS hiểu {} là block code, không phải Object -> trả về undefined
const getUser = (id) => { id: id, name: "Admin" }; 

// ĐÚNG: Bao quanh bằng () để JS hiểu đây là một biểu thức (expression)
const getUserCorrect = (id) => ({ id: id, name: "Admin" });
