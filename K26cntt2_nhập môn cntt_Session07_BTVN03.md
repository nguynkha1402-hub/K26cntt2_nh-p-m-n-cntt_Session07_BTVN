Họ và tên: Trần Nguyễn Kha

##### **Bài 3 : Tự động hóa Phân tích File Log**

Đề bài / Yêu cầu

1\. Mục tiêu

\- Làm chủ kỹ thuật kết nối luồng dữ liệu thông qua Pipeline (|) trong PowerShell.



\- Thực hiện chuỗi thao tác Đọc → Lọc → Đếm mà không tốn tài nguyên đĩa.



2\. Bối cảnh \& Vấn đề

Bạn cần thống kê nhanh có bao nhiêu sự cố lỗi "TIMEOUT" xảy ra trong ngày hôm nay từ file access.log. Yêu cầu phải đưa ra con số tổng cuối cùng trong 1 dòng lệnh PowerShell duy nhất.



3\. Yêu cầu nghiệp vụ

\- Sử dụng mô hình đường ống Pipeline (|) để dữ liệu chạy trực tiếp trên RAM, không ghi ra file trung gian.



\- Kết quả phải là một con số (Ví dụ: 42) thay vì in ra hàng nghìn dòng chữ.



4\. Ràng buộc \& Bẫy dữ liệu

Hệ thống đang chạy trên PowerShell. Nếu dùng lệnh wc -l của Linux sẽ bị báo lỗi. Trong PowerShell, lệnh đếm dòng tương đương là Measure-Object -Line.



**5. Yêu cầu đầu ra**

**Báo cáo phân tích:**

#### ***1. Vẽ sơ đồ 3 bước (stdout → | → stdin) mô tả luồng dữ liệu chạy trong câu lệnh PowerShell của bạn.***

#### ***Plaintext***

*\[Get-Content access.log] (stdout)*

&#x20;     *|*

*\[Select-String "TIMEOUT"] (stdin -> stdout)*

&#x20;    *|*

*\[Measure-Object -Line].Lines (stdin -> Con số duy nhất)*

#### **2. Viết câu lệnh PowerShell nối 3 bước (Đọc file | Lọc chữ | Đếm dòng) hoàn chỉnh, cho ra một con số duy nhất.**

*PowerShell*

*(Get-Content .\\access.log | Select-String "TIMEOUT" | Measure-Object -Line).Lines*

