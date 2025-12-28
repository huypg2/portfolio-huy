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
Socket (giao diện lập trình ứng dụng mạng) đóng vai trò là cầu nối trung gian giữa hai chương trình ứng dụng để trao đổi dữ liệu qua mạng. Về mặt kỹ thuật, một Socket là một điểm cuối (endpoint) của một liên kết giao tiếp hai chiều (two-way communication link) giữa hai chương trình đang chạy trên mạng.

Để hiểu đơn giản, hãy hình dung Socket giống như một thiết bị điện thoại:
* Để gọi điện, bạn cần có **số điện thoại** (địa chỉ IP) và **số máy lẻ** (Port number).
* Hệ điều hành cung cấp cơ chế Socket để ứng dụng "nhấc máy", "quay số" và "nói chuyện".

Một địa chỉ Socket hoàn chỉnh được cấu thành từ hai yếu tố:
1.  **IP Address (Địa chỉ IP):** Xác định máy tính cụ thể trên mạng.
2.  **Port Number (Số hiệu cổng):** Xác định ứng dụng cụ thể đang chạy trên máy tính đó (Ví dụ: Web server thường chạy cổng 80, Mail server chạy cổng 25).



## 2. Phân loại Socket trong Java
Trong ngôn ngữ lập trình Java (gói `java.net`), mô hình Socket chủ yếu dựa trên giao thức TCP/IP và được chia thành các lớp đối tượng chuyên biệt để xử lý các vai trò khác nhau trong mô hình Client-Server:

### a. ServerSocket (Phía Server - Người lắng nghe)
Lớp `ServerSocket` được thiết kế để hoạt động ở chế độ "thụ động" (passive). Nhiệm vụ duy nhất của nó là chờ đợi các yêu cầu kết nối từ bên ngoài.
* Nó không tham gia trực tiếp vào việc truyền dữ liệu.
* Khi có một Client kết nối đến, `ServerSocket` sẽ sinh ra một đối tượng `Socket` mới để phục vụ riêng cho Client đó, trong khi nó vẫn tiếp tục quay lại lắng nghe các kết nối tiếp theo.

### b. Socket (Phía Client & Giao tiếp)
Lớp `Socket` đại diện cho một kết nối thực sự (active connection).
* **Ở phía Client:** Nó được dùng để khởi tạo kết nối đến Server.
* **Ở phía Server:** Nó là kết quả trả về sau khi `accept()` thành công, dùng để gửi/nhận dữ liệu với Client cụ thể đó.
* Đây là nơi luồng dữ liệu (Stream) thực sự chảy qua.

## 3. Cơ chế hoạt động (Mô hình TCP)
Giao thức TCP (Transmission Control Protocol) là giao thức hướng kết nối (connection-oriented), đảm bảo độ tin cậy cao. Quá trình giao tiếp diễn ra theo trình tự nghiêm ngặt sau:



1.  **Binding (Gắn cổng):**
    Server khởi tạo `ServerSocket` và gắn nó với một cổng cụ thể (ví dụ: 5000). Hệ điều hành sẽ dành riêng cổng này cho ứng dụng Server.

2.  **Listening & Blocking (Lắng nghe và Chặn):**
    Server gọi phương thức `accept()`. Tại đây, luồng chương trình của Server sẽ bị **tạm dừng (block)**. Nó sẽ đứng yên ở dòng lệnh này và chờ đợi cho đến khi có một Client hợp lệ tìm đến.

3.  **Connection Request (Yêu cầu kết nối):**
    Client khởi tạo `Socket` kèm theo địa chỉ IP và Port của Server. Lệnh này gửi một gói tin yêu cầu kết nối (SYN) đến Server.

4.  **Three-way Handshake (Bắt tay 3 bước):**
    Trước khi dữ liệu được truyền, một quá trình bắt tay 3 bước diễn ra ngầm định ở tầng Transport:
    * Client gửi SYN.
    * Server nhận được, gửi lại SYN-ACK.
    * Client gửi lại ACK.
    * -> **Kết nối thiết lập thành công.** Lúc này phương thức `accept()` bên Server mới kết thúc và trả về một đối tượng `Socket`.

5.  **Data Transfer (Trao đổi dữ liệu):**
    Hai bên sử dụng I/O Streams để giao tiếp:
    * `InputStream`: Để đọc dữ liệu đối phương gửi tới.
    * `OutputStream`: Để gửi dữ liệu đi.

6.  **Closing (Đóng kết nối):**
    Khi hoàn tất, các bên gọi `close()` để giải phóng tài nguyên cổng và bộ nhớ.

## 4. Code minh họa (Server)
```java
// Server lắng nghe tại cổng 5000
ServerSocket server = new ServerSocket(5000);
System.out.println("Server đang chờ kết nối...");

// Chấp nhận kết nối từ Client
Socket socket = server.accept();
System.out.println("Client đã kết nối!");
