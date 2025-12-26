title: "Bài 5: Phân tích lỗ hổng SQL Injection (Dự án chuyên ngành)"
date: 2025-12-26T12:00:00+07:00
draft: false
tags: ["Security", "SQL"]
cover:
    image: "https://images.unsplash.com/photo-1526374965328-7f61d4dc18c5"
    alt: "Hacker Code"
---

## 1. Bản chất lỗ hổng
SQL Injection (SQLi) xảy ra khi dữ liệu đầu vào không tin cậy được cộng chuỗi trực tiếp vào câu truy vấn SQL. Điều này làm thay đổi logic của câu lệnh, cho phép kẻ tấn công thao tác với cơ sở dữ liệu trái phép.

## 2. Các dạng tấn công
* **In-band SQLi:** Kết quả trả về ngay trên giao diện web (dễ phát hiện nhất).
* **Blind SQLi:** Không trả về lỗi hay dữ liệu, hacker phải dựa vào phản ứng của server (thời gian trả lời, trang đúng/sai) để đoán dữ liệu.

## 3. Biện pháp phòng chống (Java)
Luôn sử dụng **PreparedStatement** thay vì `Statement` thường.

* **Code lỗi (Vulnerable):**
```java
String query = "SELECT * FROM users WHERE name = '" + userName + "'";
