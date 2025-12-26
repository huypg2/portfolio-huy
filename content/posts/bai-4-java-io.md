---
title: "Bài 4: Java I/O Streams - Luồng dữ liệu"
date: 2025-12-26T11:00:00+07:00
draft: false
tags: ["Java", "IO"]
cover:
    image: "https://images.unsplash.com/photo-1516116216624-53e697fedbea"
    alt: "Data Stream"
---

## 1. Khái niệm Stream
Stream (Luồng) là một dãy dữ liệu liên tục chảy từ nguồn đến đích. Trong Java, Stream được chia làm 2 loại chính dựa trên kiểu dữ liệu:
* **Byte Stream:** Xử lý dữ liệu nhị phân (ảnh, video). Lớp gốc: `InputStream`, `OutputStream`.
* **Character Stream:** Xử lý dữ liệu văn bản (text). Lớp gốc: `Reader`, `Writer`.

## 2. Buffer (Bộ đệm)
Để tăng tốc độ đọc ghi, ta thường dùng Wrapper Class như `BufferedReader`. Nó giúp đọc một khối dữ liệu lớn vào bộ nhớ đệm thay vì đọc từng byte từ ổ cứng/mạng.

## 3. Ứng dụng trong Socket
Khi nhận tin nhắn từ Client, ta thường dùng `InputStreamReader` kết hợp `BufferedReader`:

```java
InputStream is = socket.getInputStream();
// Chuyển byte thành char, sau đó đưa vào bộ đệm
BufferedReader reader = new BufferedReader(new InputStreamReader(is));

String message = reader.readLine(); // Đọc từng dòng
System.out.println("Received: " + message);
