---
title: "Bài 1: Lý thuyết về Socket và Mô hình Client-Server"
date: 2025-12-26T08:00:00+07:00
draft: false
tags: ["Java", "Network"]
cover:
    image: "https://images.unsplash.com/photo-1544197150-b99a580bb7a8"
    alt: "Network Cables"
---

## 1. Khái niệm Socket
Socket là một điểm cuối (endpoint) trong liên kết giao tiếp hai chiều giữa hai chương trình chạy trên mạng. Một socket được ràng buộc với một số cổng (port) để tầng TCP có thể định danh ứng dụng mà dữ liệu được gửi tới.

## 2. Phân loại Socket trong Java
Trong gói `java.net`, chúng ta có 2 class chính:
* **Socket:** Dùng cho phía Client để kết nối đến Server.
* **ServerSocket:** Dùng cho phía Server để lắng nghe kết nối đến.

## 3. Cơ chế hoạt động (TCP)
1.  **Server** khởi tạo `ServerSocket` tại một cổng cụ thể.
2.  **Server** gọi phương thức `accept()` và chờ đợi.
3.  **Client** khởi tạo `Socket` với IP và Port của Server.
4.  Kết nối được thiết lập (3-way handshake).
5.  Hai bên trao đổi dữ liệu qua `InputStream` và `OutputStream`.

## 4. Code minh họa (Server)
```java
// Server lắng nghe tại cổng 5000
ServerSocket server = new ServerSocket(5000);
System.out.println("Server đang chờ kết nối...");

// Chấp nhận kết nối từ Client
Socket socket = server.accept();
System.out.println("Client đã kết nối!");
