---
title: "Bài 3: So sánh giao thức TCP và UDP"
date: 2025-12-26T10:00:00+07:00
draft: false
tags: ["Theory", "Network"]
cover:
    image: "https://images.unsplash.com/photo-1607799275518-d6e690c7b0b1"
    alt: "Data Transfer"
---

## 1. TCP (Transmission Control Protocol)
* **Đặc điểm:** Hướng kết nối (Connection-oriented). Phải thiết lập kết nối trước khi truyền.
* **Độ tin cậy:** Cao. Đảm bảo gói tin đến đúng thứ tự và không bị mất (có cơ chế gửi lại).
* **Tốc độ:** Chậm hơn UDP do tốn chi phí kiểm tra lỗi.
* **Ứng dụng:** Web (HTTP), Email (SMTP), File Transfer (FTP).

## 2. UDP (User Datagram Protocol)
* **Đặc điểm:** Không hướng kết nối (Connectionless). Gửi dữ liệu đi mà không cần biết bên kia có nhận được không.
* **Độ tin cậy:** Thấp. Gói tin có thể bị mất hoặc đến sai thứ tự.
* **Tốc độ:** Rất nhanh.
* **Ứng dụng:** Streaming Video, Game Online (Real-time), VoIP.

## 3. Ví dụ Code gửi UDP (Java)
```java
DatagramSocket socket = new DatagramSocket();
String message = "Hello UDP";
byte[] buffer = message.getBytes();
InetAddress address = InetAddress.getByName("localhost");

// Đóng gói dữ liệu vào DatagramPacket
DatagramPacket packet = new DatagramPacket(buffer, buffer.length, address, 4445);
socket.send(packet); // Bắn gói tin đi
