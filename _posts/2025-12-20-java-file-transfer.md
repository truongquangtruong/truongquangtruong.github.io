---
title: "Khóa Học #06: Kỹ Thuật Truyền Tải File Qua Socket - Xử Lý Dữ Liệu Nhị Phân Và Chiến Lược Chia Khối (Chunking)"
date: 2025-12-20
tags: ["java", "socket", "file-transfer", "binary-data", "streams"]
categories: ["Java Network"]
draft: false
---

 ![Truyền tải file qua mạng](https://images.unsplash.com/photo-1544396821-4dd40b938ad3?q=80&w=2070&auto=format&fit=crop)

Chào các bạn! Sau khi đã làm chủ được ứng dụng Chat Room ở bài 5, một câu hỏi lớn đặt ra là: *Làm sao để gửi một tấm ảnh, một file PDF hay một bản nhạc qua Socket?* Không giống như gửi tin nhắn văn bản mà chúng ta có thể dùng `println`, truyền tải file yêu cầu chúng ta phải làm việc trực tiếp với **Dữ liệu nhị phân (Binary Data)**. Trong quá trình thực hiện đồ án, đây là phần mình tốn nhiều thời gian debug nhất vì chỉ cần sai lệch 1 byte, file nhận được sẽ bị hỏng (corrupted) ngay lập tức. Hôm nay, mình sẽ chia sẻ với các bạn kỹ thuật truyền file chuyên nghiệp bằng Java.

### 1. Tại sao không thể truyền file như truyền tin nhắn?
Tin nhắn văn bản sử dụng các ký tự. Khi chúng ta dùng `BufferedReader.readLine()`, Java sẽ đợi cho đến khi gặp ký tự xuống dòng (`\n`). Tuy nhiên, file nhị phân (như .exe, .jpg) có thể chứa mã byte trùng với ký tự xuống dòng ở bất kỳ đâu. Nếu bạn dùng `readLine()` để đọc file, chương trình sẽ kết thúc sớm hoặc đọc sai dữ liệu.

**Giải pháp:** Chúng ta phải quay lại với tầng thấp nhất của Java I/O mà mình đã nhắc ở Bài 2: **InputStream** và **OutputStream**.

### 2. Chiến lược chia khối (Chunking Strategy)
Một sai lầm phổ biến của các bạn sinh viên là cố gắng đọc toàn bộ file vào bộ nhớ RAM rồi mới gửi đi. 
* **Hệ quả:** Nếu file nặng 2GB mà RAM máy tính chỉ có 1GB, chương trình sẽ báo lỗi `OutOfMemoryError` lập tức.

**Kỹ thuật chuyên nghiệp:** Chúng ta sử dụng một bộ đệm nhỏ (thường là 4KB hoặc 8KB). 
1. Đọc một "miếng" file (4KB) từ ổ cứng vào bộ đệm.
2. Đẩy "miếng" đó ra Socket.
3. Lặp lại cho đến khi hết file.
Cách này giúp bạn có thể truyền file dung lượng hàng trăm GB mà chỉ tốn vài KB RAM.



### 3. Triển khai mã nguồn Truyền File "Bất bại"
Dưới đây là đoạn code mình đã viết lại theo tiêu chuẩn tối ưu, bao gồm cả việc gửi tên file và kích thước file trước khi truyền dữ liệu thật.

```java
import java.io.*;
import java.net.*;

/**
 * Hệ thống Truyền tải File Chuyên nghiệp
 * Thiết kế: Trương Quang Trưởng
 */
public class FileTransferServer {
    public static void main(String[] args) {
        final int PORT = 8888;
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("=== FILE SERVER [TRƯỞNG] ĐANG CHỜ KẾT NỐI... ===");

            while (true) {
                try (Socket clientSocket = serverSocket.accept();
                     DataInputStream dis = new DataInputStream(clientSocket.getInputStream())) {

                    // 1. Nhận thông tin Meta-data của file
                    String fileName = dis.readUTF();
                    long fileSize = dis.readLong();
                    System.out.println("[RECEIVING] Đang nhận file: " + fileName + " (" + fileSize + " bytes)");

                    // 2. Chuẩn bị luồng ghi vào ổ cứng
                    File file = new File("received_" + fileName);
                    try (FileOutputStream fos = new FileOutputStream(file)) {
                        byte[] buffer = new byte[4096]; // Bộ đệm 4KB
                        int bytesRead;
                        long totalReceived = 0;

                        // 3. Vòng lặp nhận dữ liệu theo khối (Chunking)
                        while (totalReceived < fileSize && 
                              (bytesRead = dis.read(buffer, 0, (int)Math.min(buffer.length, fileSize - totalReceived))) != -1) {
                            fos.write(buffer, 0, bytesRead);
                            totalReceived += bytesRead;
                            
                            // Hiển thị tiến độ (Progress bar)
                            int progress = (int) (totalReceived * 100 / fileSize);
                            System.out.print("\rTiến độ: " + progress + "%");
                        }
                    }
                    System.out.println("\n[SUCCESS] Đã nhận file thành công và lưu tại: " + file.getAbsolutePath());

                } catch (IOException e) {
                    System.err.println("\n[ERR] Lỗi khi nhận file: " + e.getMessage());
                }
            }
        } catch (IOException e) {
            System.err.println("[CRITICAL] Lỗi Server: " + e.getMessage());
        }
    }
}