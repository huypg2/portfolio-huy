---
title: "Bài 4: Java I/O Streams - Luồng dữ liệu"
date: 2025-12-26T11:00:00+07:00
draft: false
tags: ["Java", "IO"]
cover:
    image: "https://images.unsplash.com/photo-1516116216624-53e697fedbea"
    alt: "Data Stream"
---

## 1. Khái niệm Stream (Luồng dữ liệu)
Trong Java, **Stream** là một khái niệm trừu tượng dùng để biểu diễn dòng chảy của dữ liệu. Hãy tưởng tượng Stream giống như một "ống dẫn nước" nối từ Nguồn (Source) đến Đích (Destination).
* **Nguồn:** Có thể là bàn phím, file trên ổ cứng, hoặc dữ liệu từ mạng (Socket).
* **Đích:** Có thể là màn hình console, file lưu trữ, hoặc gửi qua mạng cho máy khác.
* **Tính chất:** Lập trình viên không cần quan tâm thiết bị thực tế là gì, chỉ cần quan tâm đến việc mở ống, cho dữ liệu chảy qua và đóng ống lại.



Java chia Stream thành hai loại chính dựa trên đơn vị dữ liệu mà nó vận chuyển:

### a. Byte Stream (Luồng Byte)
* **Đơn vị:** Xử lý dữ liệu theo từng byte (8-bit).
* **Lớp gốc (Abstract Class):** `InputStream` và `OutputStream`.
* **Ứng dụng:** Đây là dạng dữ liệu thô nhất, dùng để xử lý các file nhị phân như hình ảnh (`.jpg`, `.png`), âm thanh (`.mp3`), video hoặc dữ liệu nén. Máy tính thực chất chỉ hiểu byte, nên đây là luồng cơ bản nhất.

### b. Character Stream (Luồng Ký tự)
* **Đơn vị:** Xử lý dữ liệu theo từng ký tự (16-bit Unicode).
* **Lớp gốc (Abstract Class):** `Reader` và `Writer`.
* **Ứng dụng:** Chuyên dùng để xử lý văn bản (Text). Nó ra đời để giải quyết vấn đề bảng mã (Encoding). Nếu dùng Byte Stream để đọc văn bản tiếng Việt hay tiếng Nhật, ký tự sẽ bị lỗi (do 1 ký tự có thể cần nhiều hơn 1 byte). Character Stream tự động chuyển đổi byte sang ký tự theo bảng mã (như UTF-8).



## 2. Mô hình Wrapper và Cầu nối (Bridging)
Trong lập trình mạng, `Socket` chỉ cung cấp luồng Byte (`InputStream`), nhưng chúng ta thường muốn gửi tin nhắn văn bản (String). Java cung cấp các lớp "Cầu nối" để chuyển đổi:

* **InputStreamReader:** Nhận vào một luồng Byte -> Chuyển đổi thành luồng Character.
* **OutputStreamWriter:** Nhận vào các Character -> Chuyển đổi thành luồng Byte để gửi đi.

Đây là lý do bạn thường thấy code lồng nhau: `new InputStreamReader(socket.getInputStream())`.

## 3. Buffer (Bộ đệm) - Tối ưu hóa hiệu năng
Nếu đọc trực tiếp từ mạng hoặc ổ cứng từng byte một, tốc độ sẽ rất chậm do tốn chi phí truy xuất hệ thống (System Call).



**Giải pháp:** Sử dụng Bộ đệm (Buffer).
* **Cơ chế:** Thay vì chuyển từng viên gạch, ta dùng xe rùa chở một lúc nhiều viên. `BufferedReader` sẽ đọc một khối dữ liệu lớn (ví dụ 8KB) từ nguồn vào bộ nhớ RAM (Buffer). Khi chương trình cần dữ liệu, nó lấy ngay từ RAM cực nhanh.
* **Lợi ích:** Tăng tốc độ đọc/ghi đáng kể và cung cấp các phương thức tiện lợi như `readLine()` (đọc nguyên 1 dòng văn bản thay vì từng ký tự).

## 4. Ứng dụng trong Socket
Trong mô hình Client-Server, việc kết hợp (Chaining) các luồng là kỹ thuật tiêu chuẩn để xử lý tin nhắn dạng văn bản (Chat):

1.  Lấy luồng Byte thô từ Socket.
2.  Chuyển đổi sang luồng Character để hiểu tiếng Việt/Unicode.
3.  Bọc trong Buffer để đọc nhanh và đọc theo dòng.

```java
// 1. Lấy luồng byte từ Socket (Dữ liệu thô từ mạng)
InputStream is = socket.getInputStream();

// 2. Cầu nối: Chuyển Byte sang Character (Giải mã UTF-8 mặc định)
InputStreamReader isr = new InputStreamReader(is);

// 3. Đóng gói vào Buffer: Để đọc từng dòng (readLine)
BufferedReader reader = new BufferedReader(isr);

// Đọc dữ liệu: Chương trình sẽ dừng tại đây chờ cho đến khi nhận được ký tự xuống dòng (\n)
String message = reader.readLine(); 
System.out.println("Received: " + message);
