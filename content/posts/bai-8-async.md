---
title: "Bài 8: Xử lý bất đồng bộ - Async/Await"
date: 2025-12-26T15:00:00+07:00
draft: false
tags: ["JavaScript", "Async", "ES8"]
cover:
    image: "https://images.unsplash.com/photo-1555099962-4199c345e5dd"
    alt: "Matrix Code"
---

## 1. Bản chất: JavaScript là Đơn luồng (Single-threaded)
Trước khi hiểu Async/Await, ta phải hiểu "nỗi đau" của JS.
JavaScript engine (như V8 trong Chrome) chỉ có **một Call Stack** (Ngăn xếp thực thi). Điều này nghĩa là tại một thời điểm, nó chỉ có thể chạy đúng **một dòng lệnh**.



**Vấn đề:**
Nếu bạn thực hiện một tác vụ nặng (như gọi API lấy dữ liệu mất 5 giây, hoặc xử lý ảnh lớn) theo cách đồng bộ (Synchronous):
1.  Call Stack bị "tắc" ở dòng lệnh đó.
2.  Trình duyệt không thể phản hồi bất kỳ sự kiện nào khác (click chuột, cuộn trang, animation).
3.  -> **Giao diện bị đóng băng (Freezing)**.

**Giải pháp:** Cơ chế Bất đồng bộ (Asynchronous). JS sẽ đẩy việc nặng ra cho môi trường (Browser Web API hoặc Node C++ APIs) xử lý, còn nó tiếp tục chạy dòng lệnh tiếp theo. Khi nào việc nặng xong, kết quả sẽ được đẩy vào hàng đợi để xử lý sau.

## 2. Sự tiến hóa: Từ Callback đến Async/Await
Async/Await (ra mắt ở ES8/ES2017) không phải là công nghệ mới hoàn toàn, mà là "cú pháp ngọt" (Syntactic Sugar) bọc bên ngoài **Promise**.

1.  **Callback (Quá khứ):** Dễ gây ra "Callback Hell" (các hàm lồng nhau hình kim tự tháp) khó bảo trì.
2.  **Promise (ES6):** Giải quyết callback hell bằng `.then()` và `.catch()`. Tuy nhiên, nếu logic phức tạp, chuỗi `.then()` vẫn khá rối rắm.
3.  **Async/Await (ES8):** Giúp viết code bất đồng bộ mà cấu trúc nhìn y hệt như code đồng bộ (tuần tự từ trên xuống dưới).

## 3. Cơ chế hoạt động của Async/Await

### a. Từ khóa `async`
* Đặt trước khai báo hàm.
* Biến hàm đó thành một hàm bất đồng bộ.
* **Luôn luôn trả về một Promise**, bất kể bạn return cái gì. (Ví dụ `return 1` thì thực tế nó trả về `Promise.resolve(1)`).

### b. Từ khóa `await`
* Chỉ được dùng bên trong hàm `async` (hoặc top-level trong ES Modules).
* **Tác dụng:** Tạm dừng việc thực thi của **hàm async đó** (không chặn luồng chính của chương trình) cho đến khi Promise phía sau nó được giải quyết (Resolved) hoặc bị từ chối (Rejected).
* Nó tự động "bóc tách" vỏ Promise để lấy dữ liệu thô bên trong.



## 4. Ví dụ thực tế: Gọi API

### Code cơ bản
```javascript
// Giả lập một hàm gọi API tốn 2 giây
const fakeApiCall = () => {
    return new Promise((resolve) => {
        setTimeout(() => resolve("Dữ liệu người dùng"), 2000);
    });
};

async function main() {
    console.log("1. Bắt đầu");

    // Code sẽ TẠM DỪNG tại dòng này 2 giây
    // Nhưng giao diện web KHÔNG bị đơ, main thread vẫn rảnh để làm việc khác
    const result = await fakeApiCall(); 

    console.log("2. Kết quả:", result); 
    console.log("3. Kết thúc");
}

main();
// Output thứ tự: 
// 1. Bắt đầu
// (Chờ 2s...)
// 2. Kết quả: Dữ liệu người dùng
// 3. Kết thúc
