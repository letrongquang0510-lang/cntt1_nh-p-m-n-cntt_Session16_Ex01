
# **Yêu cầu đầu ra**

## **Phần 1. Phân tích**

### **Xác định Input (Tên miền) / Output (IP address) và mô tả sơ đồ luồng dữ liệu của DNS.**

 - input : tên miền dạng do nam nhập `phong-dieu-phoi.grennlog.vn`
- output : Địa chỉ IP dạng số phiên bản IPv4 hoặc IPv6 của máy chủ lưu trữ hệ thống điều phối 
- sơ đồ luồng dữ liệu của DNS : 
+) B1 : nam nhập  `phong-dieu-phoi.grennlog.vn`  
+) B2 : gửi truy vấn tới DNS Resolver
+) B3 : Resolver lưu Cache và gửi IP về cho trình duyệt 

## **Phần 2. Triển khai**

### **Đề xuất quy trình kiểm tra mã trạng thái HTTP Status Code để xác nhận xem yêu cầu đã chạm tới máy chủ hay chưa.**
-  quy trình 4 bước :
1.  DevTools: Nhấn F12 trên trình duyệt →\rightarrow→ Chuyển sang tab Network.
    
2.  Gửi Request : Gõ địa chỉ `phong-dieu-phoi.greenlog.vn`  →\rightarrow→ Nhấn Enter.
    
3.  Xem kết quả:Kiểm tra giá trị tại cột Status của dòng yêu cầu đầu tiên.
    
4.  Xác nhận kết nối:
    -   Chưa chạm Server: Status hiển thị `(failed)` hoặc báo lỗi DNS (`NXDOMAIN`).
        
    -   Đã chạm Server :  Status xuất hiện mã 3 chữ số cụ thể (2xx, 3xx, 4xx, 5xx).

## **Phần 3. Kiểm thử**

### **Giả định Nam sửa đúng tên miền nhưng vẫn nhận mã 404 Not Found, hệ thống đang gặp vấn đề gì?**

- việc nam gửi `phong-dieu-phoi.grennlog.vn`mà hệ thống  trả về mã  404 Not Found xác nhận kết nối mạng và đã phân giải thành công DNS , gói HTTP Request đã chạm tới Server thành công.
- nguyên nhân gọn từ phía hệ thống gây lỗi HTTP 404 Not Found :

5 nguyên nhân gọn từ phía hệ thống gây lỗi HTTP 404 Not Found :

1.  Sai cấu hình Web Server : Apache trỏ nhầm thư mục gốc (Root Directory) hoặc cấu hình sai `server_name`.
   
2.  Lỗi Triển khai (Deploy) : Bộ mã nguồn ứng dụng chưa được tải lên máy chủ hoặc bị thiếu file khởi chạy (`index.html`/`index.php`).
    
3.  Thiếu Rewrite Rule  : Web Server thiếu luật chuyển hướng `try_files` về `index.html` khi dùng các framework như React/Vue.
    
4.  Dịch vụ Backend chưa chạy : Application service (Node.js, Java, Python) hoặc Docker container ứng dụng bên trong đang bị tắt/crash.
 
6.  Sai phân quyền : Web Server không có quyền đọc (`Read`) tệp tin mã nguồn trên hệ điều hành máy chủ.
