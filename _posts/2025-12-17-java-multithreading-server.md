---
title: "Khóa Học #03: Đa Luồng (Multi-threading) - Giải Pháp Xử Lý Hàng Ngàn Kết Nối Đồng Thời & Tối Ưu Hóa Tài Nguyên"
date: 2025-12-17
tags: ["java", "multithreading", "concurrency", "server-architecture", "performance"]
categories: ["Java Network"]
draft: false
---

 ![Đồng bộ hóa luồng trong Java](https://gpcoder.com/wp-content/uploads/2018/02/object-locking-e1518363859630.png)

Chào các bạn! Sau khi chúng ta đã nắm vững kỹ thuật tối ưu "ống dẫn" dữ liệu ở bài học số 2, hôm nay chúng ta sẽ bước vào một chương đầy thử thách nhưng cũng đầy thú vị: **Multi-threading (Đa luồng)**. 

Trong quá trình thực hiện đồ án, mình đã từng đặt câu hỏi: *Tại sao các website lớn như Facebook hay Shopee có thể phục vụ hàng triệu người cùng lúc, trong khi Server Socket cơ bản của mình chỉ đón được một người là đã "nghẹt thở"?* Câu trả lời nằm ở khả năng xử lý đồng thời (Concurrency). Hôm nay mình sẽ chia sẻ toàn bộ kiến thức mình đã "vượt khó" để nâng cấp Server từ đơn luồng lên đa luồng chuyên nghiệp.

### 1. Phân tích sâu: Nỗi ám ảnh mang tên "Blocking I/O"
Ở Bài 1, mình đã giải thích về lệnh `serverSocket.accept()`. Đây là một lệnh **Blocking**. Để các bạn dễ hình dung, hãy tưởng tượng một cửa hàng chỉ có duy nhất một nhân viên phục vụ:
1. Nhân viên đứng đợi khách tại cửa (`accept`).
2. Có một khách A vào, nhân viên dẫn khách vào bàn và đứng đó đợi khách order, ăn uống, thanh toán (`read/write`).
3. Trong lúc khách A đang ăn, có 10 khách khác đang đứng ngoài cửa. Nhưng vì nhân viên đang bận phục vụ khách A, cửa hàng hoàn toàn bị "tắc nghẽn".

Trong lập trình mạng, nếu Client A kết nối mà không gửi dữ liệu, dòng lệnh `in.readLine()` sẽ treo toàn bộ Server. Đây chính là lỗ hổng khiến hệ thống của bạn dễ dàng bị tấn công từ chối dịch vụ (DoS).



### 2. Giải pháp kiến trúc: Từ Thread-per-Client đến Thread Pool
Để giải quyết, mình đã nghiên cứu hai hướng tiếp cận chính:

#### 2.1. Thread-per-Client (Cách làm truyền thống)
Mỗi khi có `accept()`, chúng ta tạo ngay một `new Thread()`. 
* **Vấn đề:** Mỗi Thread trong Java chiếm khoảng 512KB - 1MB RAM (Stack size). Nếu có 2000 kết nối, bạn mất ngay 2GB RAM chỉ để duy trì luồng. Chưa kể CPU sẽ bị quá tải vì phải liên tục chuyển đổi ngữ cảnh (Context Switching).

#### 2.2. Thread Pool (Tiêu chuẩn công nghiệp)
Thay vì tạo mới, mình sử dụng **Executor Framework**. Mình thuê sẵn một "đội đặc nhiệm" gồm 50-100 Thread rảnh rỗi. 
* Khách đến -> Giao Socket cho 1 Thread trong Pool.
* Khách đi -> Thread quay lại Pool nghỉ ngơi, chờ khách mới.
* **Lợi ích:** Kiểm soát tuyệt đối tài nguyên, Server không bao giờ bị "tràn bộ nhớ" hay sập nguồn do quá tải kết nối.



### 3. Thực thi mã nguồn Server Đa luồng "Khủng"
Đây là bộ mã nguồn mình đã tinh chỉnh để đạt độ ổn định cao nhất, tách biệt rõ ràng giữa logic lắng nghe và logic xử lý dữ liệu.

```java
import java.io.*;
import java.net.*;
import java.util.concurrent.*;
import java.util.Date;

/**
 * Hệ thống Server Đa luồng Hiệu năng cao
 * Thiết kế bởi: Trương Quang Trưởng
 */
public class HighPerformanceServer {
    public static void main(String[] args) {
        final int PORT = 8080;
        // Khởi tạo Thread Pool với 100 luồng công nhân
        ExecutorService workerPool = Executors.newFixedThreadPool(100);

        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("=== SERVER ĐA LUỒNG [TRƯỞNG] STARTING AT PORT " + PORT + " ===");
            System.out.println("[LOG] Hệ thống sẵn sàng phục vụ 100 kết nối đồng thời...");

            while (true) {
                try {
                    // Chấp nhận kết nối và lập tức giao việc để quay lại lắng nghe tiếp
                    Socket clientSocket = serverSocket.accept();
                    System.out.println("\n[NEW] Khách kết nối từ: " + clientSocket.getInetAddress());

                    // Đưa yêu cầu vào hồ chứa luồng
                    workerPool.execute(new ClientWorker(clientSocket));
                } catch (IOException e) {
                    System.err.println("[ERR] Lỗi kết nối đơn lẻ: " + e.getMessage());
                }
            }
        } catch (IOException e) {
            System.err.println("[CRITICAL] Lỗi hệ thống Server: " + e.getMessage());
        }
    }
}

/**
 * Lớp Worker xử lý riêng biệt cho từng khách hàng
 */
class ClientWorker implements Runnable {
    private final Socket clientSocket;

    public ClientWorker(Socket socket) {
        this.clientSocket = socket;
    }

    @Override
    public void run() {
        String threadName = Thread.currentThread().getName();
        try (
            BufferedReader reader = new BufferedReader(new InputStreamReader(clientSocket.getInputStream(), "UTF-8"));
            PrintWriter writer = new PrintWriter(new OutputStreamWriter(clientSocket.getOutputStream(), "UTF-8"), true)
        ) {
            writer.println("Chào mừng bạn đến với Server của Trưởng! Bạn đang được phục vụ bởi: " + threadName);

            String input;
            while ((input = reader.readLine()) != null) {
                if (input.equalsIgnoreCase("quit")) break;
                
                System.out.println("[" + threadName + "] Nhận: " + input);
                // Giả lập xử lý nặng mất 1 giây
                Thread.sleep(1000); 
                writer.println("Xử lý xong [" + input.toUpperCase() + "] vào lúc " + new Date());
            }
        } catch (Exception e) {
            System.err.println("[ERR-" + threadName + "] Lỗi phiên làm việc: " + e.getMessage());
        } finally {
            try {
                clientSocket.close();
                System.out.println("[-] " + threadName + " đã hoàn thành nhiệm vụ và đóng kết nối.");
            } catch (IOException e) { e.printStackTrace(); }
        }
    }
}