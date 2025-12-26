---
title: "Bài 2: Đa luồng (Multi-threading) trong Lập trình mạng"
date: 2025-12-26T09:00:00+07:00
draft: false
tags: ["Java", "Thread"]
cover:
    image: "https://images.unsplash.com/photo-1550751827-4bd374c3f58b"
    alt: "CPU Threads"
---

## 1. Tại sao cần Đa luồng?
Trong mô hình Client-Server, nếu Server chỉ dùng một luồng chính (Single-thread), nó chỉ có thể phục vụ **một Client tại một thời điểm**. Các Client khác phải chờ đợi (Blocking).
Để giải quyết, ta sử dụng **Multi-threading**: Mỗi khi có Client kết nối, Server sẽ tạo ra một luồng riêng để xử lý Client đó.

## 2. Vòng đời của Thread
* **New:** Thread được khởi tạo.
* **Runnable:** Sẵn sàng chạy.
* **Running:** Đang thực thi mã lệnh.
* **Blocked/Waiting:** Chờ tài nguyên hoặc tín hiệu.
* **Terminated:** Hoàn thành công việc.

## 3. Thực hiện trong Java
Cách phổ biến nhất là kế thừa lớp `Thread` hoặc implement interface `Runnable`.

```java
public class ClientHandler extends Thread {
    private Socket socket;

    public ClientHandler(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        // Xử lý logic đọc/ghi dữ liệu riêng cho client này
        System.out.println("Đang xử lý trên luồng: " + Thread.currentThread().getId());
    }
}
