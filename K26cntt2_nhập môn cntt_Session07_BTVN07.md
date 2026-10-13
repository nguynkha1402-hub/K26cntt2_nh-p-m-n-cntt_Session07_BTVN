Họ và tên: Trần Nguyễn Kha  
**Bài 7 : Debug Shell Script xử lý log CRITIC** **AL**  
Đề bài / Yêu cầu  
1. Mục tiêu  
Đọc hiểu và trace code một đoạn PowerShell Script tự động hóa xử lý log.  
Phát hiện và sửa lỗi logic nghiệp vụ khiến script hoạt động sai.  
2. Bối cảnh  
Bạn là intern tại công ty SaaS MonitorPro chuyên cung cấp dịch vụ giám sát hệ thống. Mỗi đêm, hệ thống tự động chạy script deploy_checker.ps1 để quét file server.log và tổng hợp các lỗi CRITICAL vào file daily_critical.txt gửi cho On-Call Engineer. Sáng hôm nay, kỹ sư On-Call phàn nàn: 'File daily_critical.txt gần như trống rỗng dù hệ thống rõ ràng đã bắn ra lỗi lúc 02:30 sáng!'  
3. Vấn đề hiện tại  
***Script deploy_checker.ps1 đang có như sau:***  
***# Script: deploy_checker.ps1***  
***# Mục đích: Trích xuất lỗi CRITICAL từ server.log vào daily_critical.txt***  
***"=== Bat dau quet log: $(Get-Date) ===" | Out-File daily_critical.txt***  
***Select-String -Path server.log -Pattern "critical" -CaseSensitive | Out-File -Append daily_critical.txt***  
***"=== Hoan tat. So dong loi: ===" | Out-File -Append daily_critical.txt***  
***(Get-Content daily_critical.txt).Count | Out-File -Append daily_critical.txt***  
Nội dung file server.log (trích đoạn):  
***2026-09-18 01:05:22 INFO     Service started successfully***  
***2026-09-18 02:30:11 CRITICAL Database connection pool exhausted***  
***2026-09-18 02:30:45 CRITICAL Retry limit exceeded for payment-service***  
***2026-09-18 03:00:00 WARNING  Disk usage at 85%***  
***2026-09-18 03:15:00 INFO     Health check passed***  
Kiến thức mở rộng : Trên PowerShell, Select-String mặc định không phân biệt hoa/thường (khác với thói quen một số bạn hay nghĩ). Tuy nhiên, cmdlet này có một tham số tùy chọn là -CaseSensitive — nếu vô tình thêm cờ này vào, lệnh sẽ chỉ khớp chính xác chữ hoa/thường như pattern đã gõ. Trong đoạn script trên, pattern gõ là "critical" (chữ thường) nhưng log lại ghi CRITICAL (chữ hoa) — kết hợp với cờ -CaseSensitive, lệnh sẽ không khớp được dòng nào cả, khiến phần nội dung lỗi bị bỏ trống (chỉ còn lại 2 dòng tiêu đề và dòng đếm số dòng = 2).  
**4. Yêu cầu đầu ra**  
**Phần 1: Phân tích lỗi**  
**Chạy thử script (trace code) với dữ liệu log mẫu trên và chỉ ra chính xác lý do file daily_critical.txt bị trống.**  
Do lệnh* Select-String *dùng tham số  *-CaseSensitive* khớp chính xác chữ thường " *critical*", nhưng file * server.log *lưu toàn bộ bằng chữ hoa, lệnh không lọc được dòng nào, khiến file kết quả chỉ còn các dòng tiêu đề trắng.  
**Phần 2: Sửa lỗi và kiểm thử**  
**Cung cấp đoạn script đã được sửa đúng.**  
**Viết kết quả mong đợi của file daily_critical.txt sau khi script chạy đúng.**  
**Điều gì xảy ra nếu file server.log không tồn tại? Script sẽ báo lỗi hay im lặng? Hãy mô tả bằng lời (không cần code xử lý).**  
**1. Script đã sửa đúng**  
**Xóa bỏ tham số -CaseSensitive:**  
*PowerShell*  
*"=== Bat dau quet log: $(Get-Date) ===" | Out-File daily_critical.txt*  
*Select-String -Path server.log -Pattern "critical" | Out-File -Append daily_critical.txt*  
*"=== Hoan tat. So dong loi: ===" | Out-File -Append daily_critical.txt*  
*(Get-Content daily_critical.txt).Count | Out-File -Append daily_critical.txt*  
**2. Kết quả mong đợi trong daily_critical.txt**  
*Plaintext*  
*=== Bat dau quet log: 10/12/2026 19:54:00 ===*  
*server.log:2:2026-09-18 02:30:11 CRITICAL Database connection pool exhausted*  
*server.log:3:2026-09-18 02:30:45 CRITICAL Retry limit exceeded for payment-service*  
*=== Hoan tat. So dong loi: ===*  
*5*  
**3. Trường hợp file server.log không tồn tại**  
PowerShell sẽ phát sinh lỗi màu đỏ và script dừng lại chứ không chạy ngầm im lặng.  
