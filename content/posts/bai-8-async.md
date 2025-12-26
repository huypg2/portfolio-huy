---
title: "Bài 8: Xử lý bất đồng bộ - Async/Await"
date: 2025-12-26T15:00:00+07:00
draft: false
tags: ["JavaScript", "Async"]
cover:
    image: "https://images.unsplash.com/photo-1555099962-4199c345e5dd"
    alt: "Matrix"
---

## 1. JavaScript là đơn luồng (Single-threaded)
JS chỉ có một Call Stack, nghĩa là nó chỉ làm một việc một lúc. Nếu ta gọi API và chờ server trả về (mất 2s) mà code chạy đồng bộ, toàn bộ trang web sẽ bị đơ.

## 2. Giải pháp Async/Await
Async/Await (ES8) được xây dựng dựa trên Promise, giúp viết code bất đồng bộ trông giống như đồng bộ (dễ đọc, dễ debug).

* `async`: Khai báo hàm bất đồng bộ.
* `await`: Tạm dừng hàm đó chờ Promise giải quyết xong mới chạy tiếp.

## 3. Ví dụ gọi API
```javascript
async function getUserData() {
    try {
        console.log("Bắt đầu lấy dữ liệu...");
        // Code dừng tại đây chờ fetch xong, nhưng giao diện không bị đơ
        let response = await fetch('[https://api.example.com/user](https://api.example.com/user)');
        let data = await response.json();
        console.log(data);
    } catch (error) {
        console.log("Lỗi mạng:", error);
    }
}
