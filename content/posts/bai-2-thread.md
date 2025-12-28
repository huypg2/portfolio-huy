---
title: "Bài 2: Đa luồng (Multi-threading) trong Lập trình mạng"
date: 2025-12-26T09:00:00+07:00
draft: false
tags: ["Java", "Thread"]
cover:
    image: "https://images.unsplash.com/photo-1550751827-4bd374c3f58b"
    alt: "CPU Threads"
---

## 1. Tại sao cần Đa luồng (Multi-threading)?

### a. Vấn đề của Server đơn luồng (Single-thread)
Trong mô hình lập trình mạng cơ bản, Server thường chạy trên một luồng chính (Main Thread). Khi Server chấp nhận một kết nối bằng lệnh `accept()`, nó sẽ chuyển sang xử lý dữ liệu cho Client đó.
* **Cơ chế Blocking (Chặn):** Vì dòng lệnh chạy tuần tự từ trên xuống dưới, nếu Server đang bận đọc dữ liệu hoặc tính toán cho Client A, nó sẽ không thể quay lại lệnh `accept()` để đón Client B.
* **Hậu quả:** Client B, C, D... phải xếp hàng chờ đợi vô thời hạn cho đến khi Client A ngắt kết nối. Điều này không thể chấp nhận được trong thực tế.

### b. Giải pháp Đa luồng (Concurrency)
Để giải quyết vấn đề trên, chúng ta áp dụng mô hình **Concurrent Server** (Server đồng thời) sử dụng Đa luồng.
* **Nguyên lý:** Luồng chính (Main Thread) chỉ làm đúng một nhiệm vụ: Đứng ở cửa (cổng mạng), chờ khách đến (`accept`), và ngay lập tức giao khách đó cho một nhân viên riêng biệt (`Thread` con) phục vụ.
* **Kết quả:** Luồng chính ngay lập tức được giải phóng để quay lại đón Client tiếp theo. Các Client được phục vụ song song cùng lúc (hoặc gần như cùng lúc nhờ cơ chế phân chia thời gian của CPU).



## 2. Vòng đời của Thread (Thread Lifecycle)
Một luồng trong Java không chỉ đơn giản là chạy và dừng, nó trải qua một quy trình quản lý trạng thái nghiêm ngặt bởi **Thread Scheduler** (Bộ lập lịch của JVM):



1.  **New (Khởi tạo):**
    Luồng được tạo ra (ví dụ: `new Thread()`) nhưng chưa bắt đầu chạy. Nó mới chỉ được cấp phát bộ nhớ, chưa có tài nguyên CPU.

2.  **Runnable (Sẵn sàng):**
    Sau khi gọi phương thức `.start()`, luồng chuyển sang trạng thái Runnable. Lưu ý: **Nó chưa chạy ngay lập tức**. Nó nằm trong hàng đợi (Pool) và chờ Bộ lập lịch (Scheduler) chọn để cấp phát CPU.

3.  **Running (Đang chạy):**
    Bộ lập lịch chọn luồng từ trạng thái Runnable và cho phép nó thực thi mã lệnh trong hàm `run()`. Đây là lúc CPU thực sự làm việc.

4.  **Blocked/Waiting (Bị chặn/Chờ đợi):**
    Đây là trạng thái phổ biến nhất trong lập trình mạng. Luồng sẽ mất quyền sử dụng CPU và đứng chờ khi:
    * Chờ nhập xuất dữ liệu (I/O Blocking): Ví dụ đang đợi Client gửi tin nhắn tới (`inputStream.read()`).
    * Chờ tài nguyên bị khóa (Locked): Đợi luồng khác nhả tài nguyên dùng chung.
    * Ngủ (`Thread.sleep()`) hoặc chờ tín hiệu (`wait()`).
    * *Khi tác vụ chờ hoàn tất, luồng quay lại trạng thái Runnable (không quay lại Running ngay).*

5.  **Terminated (Kết thúc):**
    Luồng hoàn thành xong công việc trong hàm `run()` hoặc bị ngắt do lỗi ngoại lệ (Exception). Một khi đã chết, luồng không thể khởi động lại (start) được nữa.

## 3. Thực hiện trong Java
Trong Java, có hai cách chính để tạo luồng, nhưng trong lập trình mạng, chúng ta thường tách biệt logic xử lý Client ra một class riêng.

### Cách tiếp cận: Kế thừa Thread hoặc Implement Runnable
* **Kế thừa class Thread:** Đơn giản, viết code nhanh.
* **Implement interface Runnable:** Linh hoạt hơn (vì Java cho phép implement nhiều interface nhưng chỉ kế thừa 1 class), phù hợp để chia sẻ tài nguyên giữa các luồng.

Dưới đây là ví dụ sử dụng cách kế thừa `Thread` để tạo ra một "nhân viên" xử lý riêng cho từng Client:

```java
public class ClientHandler extends Thread {
    private Socket socket;

    // Nhận Socket của Client cụ thể khi khởi tạo
    public ClientHandler(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            // Đây là nơi luồng này "sống" và làm việc
            // Mọi thao tác I/O (đọc/ghi) với Client này sẽ nằm ở đây
            // để không ảnh hưởng đến luồng chính.
            
            System.out.println("Đang xử lý Client trên luồng: " + Thread.currentThread().getId());
            
            // Giả lập xử lý công việc
            // InputStream input = socket.getInputStream();
            // ... logic đọc ghi ...
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
