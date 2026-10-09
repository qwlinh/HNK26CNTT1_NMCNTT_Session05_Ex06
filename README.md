# 1. Phân tích luồng IPO (Input – Process – Output)
Plaintext
INPUT: Yêu cầu khởi tạo cơ sơ hạ tầng thiết bị Smart Farm 


PROCESS:
- Dùng New-Item tạo cây thư mục lồng nhau smart-farm với các thư mục con bin, config, data từ thư mục Home (~).
- Tạo file ẩn .env bên trong thư mục config.
- Copy file template.ps1 từ Public sang bin và đổi tên thành pump_control.ps1 trong 1 lệnh duy nhất.
- Kiểm tra, xác thực bằng lệnh liệt kê tree/ Get-ChildItem -Recurse.


OUTPUT: 
- Cấu trúc thư mục smart-farm chứa bin, config, data được thiết lập chuẩn xác.
- File ẩn .env được tạo thành công trong config.
- Bản sao file template.ps1 được đổi tên thành pump_control.ps1 nằm gọn trong thư mục bin.
- Báo cáo cấu trúc toàn bộ cây thư mục qua lệnh kiểm tra đệ quy.
                   
                        
# 2. Nhật ký (Log) các câu lệnh PowerShell thực thi:
Bạn lần lượt chạy các câu lệnh sau trên Terminal PowerShell:

Bước 1: Từ thư mục Home (~), tạo thư mục smart-farm chứa bin, config, data bằng 1 dòng lệnh
PowerShell
cd ~
New-Item -ItemType Directory -Force -Path "smart-farm\bin", "smart-farm\config", "smart-farm\data"


Bước 2: Tạo file ẩn .env trong config
PowerShell
New-Item -ItemType File -Force -Path "smart-farm\config\.env"
## Gán thuộc tính ẩn cho file .env (nếu cần thiết lập rõ ràng):
Set-ItemProperty -Path "smart-farm\config\.env" -Name Attributes -Value Hidden
Giải thích tại sao lệnh Get-ChildItem thông thường không nhìn thấy file .env:

Trên hệ điều hành Windows, các tệp bắt đầu bằng dấu chấm (.) hoặc có thuộc tính hệ thống/ẩn (Hidden) sẽ bị bộ lọc mặc định của PowerShell ẩn đi để tránh làm rối màn hình hoặc vô tình chỉnh sửa file cấu hình nhạy cảm. Do đó, để nhìn thấy file .env, bạn bắt buộc phải dùng tham số bổ sung như Get-ChildItem -Force hoặc Get-ChildItem -Hidden.


Bước 3: Sao chép file mẫu template.ps1 từ thư mục Public vào bin và đổi tên thành pump_control.ps1 chỉ trong 1 lệnh duy nhất
PowerShell
Copy-Item -Path "C:\Users\Public\template.ps1" -Destination "smart-farm\bin\pump_control.ps1"


Bước 4: Kiểm tra lại toàn bộ cấu trúc thư mục smart-farm
PowerShell
Get-ChildItem -Path "smart-farm" -Recurse -Force
# 3. Xử lý ràng buộc lỗi "Access to the path is denied"
Nếu gặp lỗi quyền hạn (Access to the path is denied) khi thực hiện thao tác sao chép tệp vào thư mục hệ thống hoặc phân vùng được bảo vệ, phương án xử lý trên Windows gồm:

 - Khởi chạy lại PowerShell với quyền Administrator: Mở menu Start, gõ PowerShell, nhấp chuột phải chọn Run as Administrator.

 - Sử dụng lệnh kích hoạt nhanh quyền Admin từ cửa sổ hiện tại (nếu hỗ trợ UAC):

PowerShell
Start-Process powershell -Verb RunAs
