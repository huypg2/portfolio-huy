---
title: "Bài 3: So sánh giao thức TCP và UDP"
date: 2025-12-26T10:00:00+07:00
draft: false
tags: ["Theory", "Network"]
cover:
    image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTRLI_CJ9TiLPUR_JlLyn-nMM_UXGolYUkgEA&s"
    alt: "Data Transfer Network"
---

Trong tầng Giao vận (Transport Layer) của mô hình TCP/IP, hai giao thức cốt lõi đóng vai trò vận chuyển dữ liệu là **TCP** và **UDP**. Việc lựa chọn giữa chúng phụ thuộc hoàn toàn vào yêu cầu của ứng dụng về **tốc độ** hay **độ tin cậy**.



## 1. TCP (Transmission Control Protocol) - Giao thức điều khiển truyền vận
TCP được ví như một cuộc gọi điện thoại: Bạn phải nhấc máy, chờ bên kia bắt máy (kết nối) thì mới nói chuyện được.

* **Cơ chế hoạt động:**
    * **Hướng kết nối (Connection-oriented):** Trước khi truyền bất kỳ byte dữ liệu nào, TCP buộc phải thiết lập một kênh truyền thông qua quy trình "Bắt tay 3 bước" (3-way handshake). Điều này đảm bảo cả hai bên đều sẵn sàng.
    * **Luồng dữ liệu (Byte Stream):** Dữ liệu được coi là một dòng chảy liên tục, không giới hạn kích thước (mặc dù bên dưới nó vẫn chia thành các segment).

* **Độ tin cậy (Reliability):**
    * **Đảm bảo thứ tự:** Gói tin gửi trước sẽ đến trước. Nếu gói tin đến sai thứ tự, TCP sẽ sắp xếp lại.
    * **Không mất mát:** Khi gửi một gói tin, bên gửi sẽ chờ tín hiệu xác nhận (ACK - Acknowledgment) từ bên nhận. Nếu không thấy ACK, nó sẽ tự động **gửi lại (retransmit)**.
    * **Kiểm soát luồng (Flow Control):** Nếu bên nhận xử lý không kịp, TCP sẽ yêu cầu bên gửi chậm lại để tránh tràn bộ đệm.

* **Nhược điểm:** Tốc độ chậm hơn và tốn tài nguyên hơn do phần đầu gói tin (Header) lớn (20 bytes) và phải xử lý nhiều cơ chế kiểm tra lỗi.

* **Ứng dụng thực tế:** Những nơi cần sự chính xác tuyệt đối như: Lướt Web (HTTP/HTTPS), Gửi Email (SMTP), Tải file (FTP), Chat văn bản (WhatsApp, Messenger).

## 2. UDP (User Datagram Protocol) - Giao thức gói dữ liệu người dùng
UDP được ví như việc gửi thư qua bưu điện hoặc phát loa thông báo: Bạn cứ gửi đi, còn người nhận có nhận được hay không, hoặc nhận lúc nào thì không chắc chắn.

* **Cơ chế hoạt động:**
    * **Không hướng kết nối (Connectionless):** Không cần bắt tay, không cần thiết lập kênh. Có dữ liệu là "bắn" đi ngay lập tức (Fire and forget).
    * **Dạng gói (Datagram):** Dữ liệu được đóng gói thành các gói tin độc lập. Mỗi gói tin tự tìm đường đi, có thể đến đích theo các con đường khác nhau.

* **Đặc điểm (Best-effort delivery):**
    * **Không đảm bảo:** UDP nỗ lực gửi đi nhanh nhất có thể, nhưng không cam kết gói tin sẽ đến nơi, không cam kết đến đúng thứ tự.
    * **Header siêu nhẹ:** Phần đầu gói tin chỉ tốn 8 bytes (so với 20 bytes của TCP), giúp giảm tải băng thông đáng kể.

* **Tại sao vẫn dùng UDP?**
    * Trong các ứng dụng thời gian thực (Real-time), **tốc độ** quan trọng hơn **sự hoàn hảo**.
    * *Ví dụ:* Khi xem Livestream hoặc chơi game FPS, nếu mất một vài khung hình (frame), người dùng chỉ thấy giật nhẹ rồi thôi. Nhưng nếu dùng TCP, video sẽ bị dừng lại (buffering) để chờ tải lại khung hình bị mất đó, gây trải nghiệm rất tệ.

* **Ứng dụng thực tế:** Gọi video (Zoom, Skype), Streaming (YouTube, Netflix - phần truyền tải media), Game Online, Hệ thống tên miền (DNS).

## 3. Bảng so sánh tóm tắt

| Đặc điểm | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
| :--- | :--- | :--- |
| **Loại kết nối** | Connection-oriented (Có kết nối) | Connectionless (Không kết nối) |
| **Độ tin cậy** | Cao (Cam kết nhận đủ, đúng thứ tự) | Thấp (Có thể mất, sai thứ tự) |
| **Cơ chế gửi lại** | Có (Nếu mất gói tin sẽ gửi lại) | Không (Mất là mất luôn) |
| **Tốc độ** | Chậm hơn | Rất nhanh |
| **Header Size** | 20 bytes | 8 bytes |
| **Cách truyền** | Luồng (Stream) | Gói (Datagram) |

## 4. Ví dụ Code gửi UDP (Java)
Khác với TCP dùng `Socket` và `ServerSocket`, UDP sử dụng `DatagramSocket` để mở cổng và `DatagramPacket` để đóng gói dữ liệu (như bỏ thư vào bao bì).

```java
import java.net.*;
import java.io.*;

public class UDPSender {
    public static void main(String[] args) {
        try {
            // Tạo socket để gửi dữ liệu (không cần chỉ định cổng gửi cụ thể, OS tự chọn)
            DatagramSocket socket = new DatagramSocket();
            
            String message = "Hello UDP, I am flying!";
            byte[] buffer = message.getBytes();
            
            // Xác định địa chỉ người nhận (Server)
            InetAddress address = InetAddress.getByName("localhost");
            int port = 4445; // Cổng mà Server UDP đang lắng nghe

            // Đóng gói dữ liệu vào DatagramPacket (Dữ liệu + Địa chỉ đích + Cổng đích)
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length, address, port);
            
            // Bắn gói tin đi (Không cần connect trước)
            socket.send(packet); 
            System.out.println("Đã gửi gói tin UDP đến " + address + ":" + port);
            
            socket.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
