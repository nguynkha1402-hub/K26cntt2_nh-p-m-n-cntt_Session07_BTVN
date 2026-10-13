Họ và tên: Trần Nguyễn Kha  
**Bài 1 : Kiểm tra File Log Máy chủ Dung Lượng Lớn**  
Đề bài / Yêu cầu  
1. Mục tiêu  
- Vận dụng đúng lệnh đọc file theo luồng (Stream) để tránh tràn RAM.  
- Phân biệt cú pháp lệnh đọc file trong PowerShell so với CMD và Git Bash.  
2. Bối cảnh & Vấn đề  
Nam là thực tập sinh tại Shopee. Hệ thống gặp sự cố lúc 12h đêm, Sếp yêu cầu Nam kiểm tra file server_log_500MB.log. Nam định mở file bằng Notepad nhưng máy tính bị treo cứng (Not Responding).  
3. Hiện trường giả  
Nam cố gắng dùng Terminal để xem 10 dòng cuối của lỗi nhưng gõ lệnh sau trên PowerShell:  
cat server_log_500MB.log  
Lệnh chạy nhưng toàn bộ 500MB nội dung đổ thẳng ra màn hình một lúc, Terminal bị đơ và không tìm được dòng lỗi.  
*Lưu ý: Trong PowerShell, cat là alias của Get-Content — lệnh này in toàn bộ nội dung ra màn hình ngay lập tức, không dừng lại theo từng trang như less trên Linux/Git Bash*  
4. Ràng buộc & Bẫy dữ liệu  
Gợi ý (kiến thức mở rộng — được giải thích sẵn tại đây): PowerShell không có sẵn lệnh phân trang riêng như less, nhưng có thể tận dụng lệnh more của Windows (đã học ở Lesson 01 cho CMD) bằng cách nối qua Pipe (đã học ở Lesson 02): Get-Content server_log_500MB.log | more. Cách này cho phép xem từng trang màn hình một, giống hệt less/more.  
5. Yêu cầu đầu ra  
***Phần 1 (Phân tích):***  
***Giải thích tại sao lệnh cat của Nam hiển thị quá nhiều dữ liệu và tại sao không nên mở file 500MB bằng Notepad.***  
-Lệnh *cat (Get-Content)*: Đọc và đổ toàn bộ nội dung file 500MB ra màn hình terminal cùng lúc mà không phân trang, gây nghẽn bộ đệm và treo ứng dụng.  
-Notepad: Cố gắng nạp toàn bộ file 500MB vào RAM ngẫu nhiên, làm vượt quá giới hạn bộ nhớ dẫn đến treo cứng máy.  
***Phần 2 (Hoàn thiện):***  
***Đưa ra lệnh PowerShell để xem toàn bộ file theo từng trang (từng màn hình một) — dựa trên gợi ý ở mục 4.***  
*PowerShell*  
*Get-Content server_log_500MB.log | more*  
***Đưa ra lệnh PowerShell để chỉ xem 10 dòng cuối cùng nhằm tìm lỗi mới nhất***  
*PowerShell*  
*Get-Content server_log_500MB.log -Tail 10*  
