Họ và tên: Trần Nguyễn Kha  
**Bài 2 : Phân tích Lỗi 404 từ File Log Máy chủ**  
Đề bài / Yêu cầu  
**1. Mục tiêu**  
- Phân biệt bản chất giữa "Tìm vị trí file" và "Tìm nội dung trong file".  
- Vận dụng lệnh Select-String trong PowerShell để chắt lọc thông tin từ file log.  
**2. Bối cảnh & Vấn đề**  
Hệ thống báo có hàng nghìn khách hàng gặp lỗi 404 (Không tìm thấy trang). Sếp yêu cầu An: "Lọc ngay các dòng chứa chữ ERROR và 404 trong file log để báo cáo".  
**3. Hiện trường giả**  
An sử dụng lệnh sau trên PowerShell:  
Get-ChildItem -Filter "*404*"  
Kết quả trả về trống rỗng dù trong file log có rất nhiều lỗi 404.  
**4. Yêu cầu đầu ra**  
***P*** ***hần 1 (Phân tích):***  
***1*** ***. Chỉ rõ sai lầm của An về bản chất câu lệnh tìm kiếm: phân biệt Get-ChildItem và Select-String trong PowerShell*** ***.***  
-Get-ChildItem: Dùng để tìm tên file/thư mục (cấu trúc bên ngoài), không quét nội dung bên trong.  
-Select-String: Dùng để tìm kiếm chuỗi văn bản nội dung sâu bên trong file.  
-Sai lầm: An dùng Get-ChildItem nên hệ thống đi tìm file có tên chứa "404" thay vì tìm nội dung lỗi bên trong file log.  
***Phần 2 (Sửa lỗi):***  
***2. Viết lại câu lệnh PowerShell chuẩn để lọc các dòng chứa chữ "ERROR" từ file access.log, phải hiển thị kèm số dòng để kỹ sư dễ tra cứu*** ***.***  
*PowerShell*  
*Select-String -Path access.log -Pattern "ERROR"*  
