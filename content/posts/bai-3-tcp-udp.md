---
title: "Bài 3: So sánh giao thức TCP và UDP"
date: 2025-12-26T10:00:00+07:00
draft: false
tags: ["Theory", "Network"]
cover:
    image: "http://googleusercontent.com/image_collection/image_retrieval/907668934813350893_0"
    alt: "Data Transfer Network"
---

## 1. TCP (Transmission Control Protocol)
* **Đặc điểm:** Hướng kết nối (Connection-oriented). Phải thiết lập kết nối (bắt tay 3 bước) trước khi truyền.
* **Độ tin cậy:** Cao. Đảm bảo gói tin đến đúng thứ tự và không bị mất (có cơ chế Acknowledgment và gửi lại).
* **Tốc độ:** Chậm hơn UDP do tốn chi phí thiết lập và kiểm tra lỗi.
* **Ứng dụng:** Web (HTTP/HTTPS), Email (SMTP), File Transfer (FTP).

## 2. UDP (User Datagram Protocol)
* **Đặc điểm:** Không hướng kết nối (Connectionless). Gửi dữ liệu đi mà không cần biết bên kia có nhận được hay không.
* **Độ tin cậy:** Thấp. Gói tin có thể bị mất hoặc đến sai thứ tự.
* **Tốc độ:** Rất nhanh do header nhỏ gọn và không cần chờ phản hồi.
* **Ứng dụng:** Streaming Video, Game Online (Real-time), VoIP, DNS.

## 3. Ví dụ Code gửi UDP (Java)

```java
import java.net.*;
import java.io.*;

public class UDPSender {
    public static void main(String[] args) {
        try {
            DatagramSocket socket = new DatagramSocket();
            String message = "Hello UDP";
            byte[] buffer = message.getBytes();
            InetAddress address = InetAddress.getByName("localhost");

            // Đóng gói dữ liệu vào DatagramPacket
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length, address, 4445);
            
            socket.send(packet); // Bắn gói tin đi
            System.out.println("Đã gửi gói tin UDP!");
            
            socket.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
