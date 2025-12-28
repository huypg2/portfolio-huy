---
title: "Bài 5: Phân tích lỗ hổng SQL Injection (Dự án chuyên ngành)"
date: 2025-12-26T12:00:00+07:00
draft: false
tags: ["Security", "SQL"]
cover:
  image: "https://images.unsplash.com/photo-1526374965328-7f61d4dc18c5"
  alt: "Hacker Code"
---

## 1. Bản chất lỗ hổng
[Image of SQL Injection diagram explanation]
SQL Injection (SQLi) xảy ra khi dữ liệu đầu vào không tin cậy từ người dùng được cộng chuỗi trực tiếp vào câu truy vấn SQL. Điều này làm thay đổi logic của câu lệnh gốc, cho phép kẻ tấn công thao tác với cơ sở dữ liệu trái phép (xem, sửa, xóa dữ liệu).

## 2. Các dạng tấn công phổ biến
* **In-band SQLi:** Kết quả trả về ngay trên giao diện web (Classic, Error-based). Dễ phát hiện nhất.
* **Blind SQLi:** Server không trả về lỗi hay dữ liệu trực tiếp. Hacker phải dựa vào phản ứng của server (thời gian trả lời - Time based, hoặc trang thái đúng/sai - Boolean based) để dò tìm dữ liệu từng ký tự một.

## 3. Biện pháp phòng chống (Java)
Luôn sử dụng **PreparedStatement** (Query Parameterization) thay vì `Statement` thường để tách biệt dữ liệu và mã lệnh.

### Code lỗi (Vulnerable):
Dữ liệu `userName` được cộng trực tiếp vào chuỗi. Nếu user nhập `' OR '1'='1` thì sẽ đăng nhập thành công mà không cần password.

```java
// NGUY HIỂM: KHÔNG ĐƯỢC DÙNG
String userName = request.getParameter("user");
String query = "SELECT * FROM users WHERE name = '" + userName + "'"; 

Statement statement = connection.createStatement();
ResultSet resultSet = statement
