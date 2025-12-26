---
title: "Bài 7: Arrow Function và từ khóa 'this'"
date: 2025-12-26T14:00:00+07:00
draft: false
tags: ["JavaScript", "Function"]
cover:
    image: "https://images.unsplash.com/photo-1587620962725-abab7fe55159"
    alt: "Coding"
---

## 1. Cú pháp Arrow Function
Đây là cách viết hàm ngắn gọn hơn trong ES6. Giúp code sạch và dễ đọc hơn.

```javascript
// Cách cũ
const sum = function(a, b) {
    return a + b;
}

// Arrow Function
const sum = (a, b) => a + b;
