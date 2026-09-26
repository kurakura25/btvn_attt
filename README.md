# Bài tập về nhà môn An toàn và bảo mật thông tin

## Họ và tên: Từ Văn Hải
## Lớp: K59.KMT.K01

1. 

Thuật toán mã hóa DES (Data Encryption Standard)

Được IBM phát triển và chính phủ Mỹ áp dụng làm tiêu chuẩn vào năm 1977, DES hiện tại đã bị coi là lỗi thời và không còn an toàn do độ dài khóa quá ngắn.

Kích thước khối (Block size): 64-bit (chia dữ liệu thành các cục 64-bit để mã hóa).

Kích thước khóa (Key size): 64-bit, nhưng 8 bit được dùng để kiểm tra lỗi (parity), do đó khóa thực tế chỉ dài 56-bit.


Cấu trúc lõi: Mạng Feistel (Feistel Network) với 16 vòng (rounds).



Quy trình mã hóa tổng thể



* Hoán vị ban đầu (Initial Permutation - IP): Đổi chỗ các bit trong khối 64-bit đầu vào theo một bảng định sẵn.



* Tách khối: Khối dữ liệu 64-bit sau IP được chia đôi thành hai nửa 32-bit:



#### Nửa Trái: L_0

#### Nửa Phải: R_0



* 16 Vòng lặp Feistel: Dữ liệu biến đổi liên tục qua 16 vòng xử lý. Tráo đổi nửa khối (Swap): Sau vòng 16, hai nửa L16 và R16 được hoán đổi vị trí thành R16L16.



* Hoán vị kết thúc (Final Permutation - FP): Áp dụng phép hoán vị ngược IP^-1 lên chuỗi R16L16 để thu được bản mã 64-bit cuối cùng.



Chi tiết cấu trúc một vòng Feistel

Tại mỗi vòng thứ i (với i từ 1 đến 16), thuật toán sử dụng khóa con Ki (48-bit) và biến đổi dữ liệu theo công thức:L_i = R_{i-1}R_i = L_{i-1} \\oplus F(R_{i-1}, Ki)

Trong đó $\\oplus$ là phép toán XOR, và $F$ là Hàm Feistel (F-Function) — thành phần quan trọng nhất tạo nên tính phi tuyến tính của DES.

