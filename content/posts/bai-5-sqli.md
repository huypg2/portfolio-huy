---
title: "Bài 5: Phân tích lỗ hổng SQL Injection (Dự án chuyên ngành)"
date: 2025-12-26T12:00:00+07:00
draft: false
tags: ["Security", "SQL"]
cover:
    image: "https://images.unsplash.com/photo-1526374965328-7f61d4dc18c5"
    alt: "Hacker Code"
---

## 1. Bản chất lỗ hổng SQL Injection (SQLi)
SQL Injection là một kỹ thuật tấn công mã (Code Injection), nơi hacker lợi dụng việc kiểm soát dữ liệu đầu vào không chặt chẽ để chèn các đoạn mã SQL độc hại vào câu truy vấn của ứng dụng.



**Tại sao nó xảy ra?**
Về mặt bản chất, nguyên nhân cốt lõi là do ứng dụng **không phân biệt được đâu là "mã lệnh" (Code) và đâu là "dữ liệu" (Data)**.
* Khi cộng chuỗi (`String concatenation`), dữ liệu người dùng nhập vào được gộp chung với câu lệnh SQL gốc.
* Hệ quản trị CSDL (như MySQL, SQL Server) sẽ biên dịch toàn bộ chuỗi này. Nếu người dùng nhập vào các ký tự đặc biệt (như `'`, `--`, `;`), họ có thể "thoát" khỏi vùng dữ liệu và biến input của mình thành một phần của mã lệnh thực thi.

**Hậu quả:**
* **Bypass Authentication:** Đăng nhập không cần mật khẩu.
* **Data Exfiltration:** Đánh cắp dữ liệu nhạy cảm (User, Password, thẻ tín dụng).
* **Data Integrity:** Sửa đổi hoặc xóa dữ liệu trong bảng.
* **Denial of Service:** Xóa bảng (DROP TABLE) gây sập hệ thống.

## 2. Các dạng tấn công phổ biến
Dựa vào cách thức phản hồi của Server, SQLi được chia thành các loại chính:

### a. In-band SQLi (Cổ điển)
Đây là dạng phổ biến nhất, hacker sử dụng cùng một kênh liên lạc để tấn công và nhận kết quả.
* **Error-based:** Hacker cố tình nhập sai cú pháp để Database trả về thông báo lỗi. Thông báo này vô tình tiết lộ thông tin về cấu trúc bảng, tên database hoặc phiên bản hệ điều hành.
* **Union-based:** Sử dụng toán tử `UNION` để gộp kết quả của câu truy vấn gốc với kết quả của câu truy vấn do hacker chèn vào. Dữ liệu bị đánh cắp sẽ hiện ngay trên giao diện web.

### b. Blind SQLi (SQLi Mù)
Nguy hiểm hơn vì Server không trả về lỗi hay dữ liệu trực tiếp. Hacker phải "mò mẫm" bằng cách đặt câu hỏi Đúng/Sai cho Database.
* **Boolean-based:** Dựa vào việc trang web trả về nội dung khác nhau (True/False) khi thay đổi câu truy vấn để đoán từng ký tự của mật khẩu/dữ liệu.
* **Time-based:** Hacker chèn lệnh yêu cầu Database "ngủ" (Ví dụ: `SLEEP(10)` trong MySQL). Nếu trang web phản hồi chậm đi đúng 10 giây, nghĩa là câu lệnh SQL đã được thực thi -> Suy ra cấu trúc Database.

## 3. Phân tích Code lỗi (Vulnerable Code)
Dưới đây là ví dụ điển hình của việc cộng chuỗi gây ra lỗ hổng.

```java
// NGUY HIỂM: KHÔNG ĐƯỢC DÙNG TRONG THỰC TẾ
String userName = request.getParameter("user"); 
// Giả sử user nhập: admin' OR '1'='1

String query = "SELECT * FROM users WHERE name = '" + userName + "'"; 

// Câu lệnh thực tế khi chạy sẽ thành:
// SELECT * FROM users WHERE name = 'admin' OR '1'='1'

Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery(query);
