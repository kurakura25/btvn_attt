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


### Thuật toán AES (Advanced Encryption Standard)
AES (hay Rijndael) ra đời năm 2001 để thay thế DES. Đây là tiêu chuẩn mã hóa hiện đại, cực kỳ an toàn và đang được sử dụng phổ biến nhất trên toàn thế giới (từ Wifi, SSL/TLS, đến mã hóa tệp tin).

- Cấu trúc: Sử dụng Mạng Thay thế - Hoán vị (Substitution-Permutation Network), xử lý song song toàn bộ khối thay vì chia làm hai nửa như DES.

- Kích thước: Khối dữ liệu cố định 128-bit. Độ dài khóa có thể là 128-bit (10 vòng lặp), 192-bit (12 vòng lặp), hoặc 256-bit (14 vòng lặp).

Quy trình mã hóa (Ví dụ khóa 128-bit):

Mở rộng khóa (Key Expansion / Key Schedule)

Khóa bí mật ban đầu của bạn (128, 192, hoặc 256 bit) không được dùng trực tiếp cho mọi vòng. Thay vào đó, nó được dùng để "sinh ra" một danh sách các khóa phụ (Round Keys) thông qua một lịch trình khóa.

AES-128 cần 11 khóa (1 khóa khởi tạo + 10 khóa vòng).

Quá trình này sử dụng các phép hoán vị bit, thay thế qua S-box và XOR với hằng số vòng (Rcon) để đảm bảo nếu khóa gốc thay đổi 1 bit, toàn bộ các khóa phụ sẽ thay đổi hoàn toàn.

- Dữ liệu được sắp xếp thành một ma trận 4x4 byte gọi là State.

- Bước khởi tạo: AddRoundKey - XOR ma trận dữ liệu với khóa bí mật ban đầu.

- 9 Vòng lặp chính: Mỗi vòng đi qua 4 phép biến đổi:

+ SubBytes: Thay thế từng byte trong ma trận bằng một byte khác dựa trên bảng tra cứu (S-box) để tạo sự xáo trộn phức tạp.

Mỗi byte (8 bit) trong ma trận State được lấy ra và thay thế bằng một byte hoàn toàn khác thông qua một bảng tra cứu cố định gọi là Rijndael S-box. S-box này được thiết kế cẩn thận bằng toán học (nghịch đảo trong trường Galois) để chống lại các cuộc tấn công vi phân và tuyến tính.

+ ShiftRows: Dịch vòng các byte trong mỗi hàng (Hàng 1 giữ nguyên, hàng 2 dịch trái 1 byte, hàng 3 dịch 2 byte, hàng 4 dịch 3 byte).
Dữ liệu trong các hàng của ma trận được dịch vòng (cyclically shift) sang bên trái.

Hàng 0: Giữ nguyên.

Hàng 1: Dịch trái 1 byte.

Hàng 2: Dịch trái 2 byte.

Hàng 3: Dịch trái 3 byte.
Mục đích: Ngăn chặn việc một cột dữ liệu bị mã hóa độc lập. Dữ liệu từ một cột ban đầu sẽ bị rải đều sang 4 cột khác nhau.

+ MixColumns: Áp dụng phép toán ma trận trộn lẫn dữ liệu theo từng cột để tạo sự khuếch tán.
Phép toán phức tạp nhất trong AES. Mỗi cột 4-byte của State được xem như một đa thức bậc 3. Cột này sẽ được nhân với một ma trận biến đổi cố định trong trường hữu hạn Galois GF($2^8$).

$$\begin{bmatrix} s'_{0,c} \\ s'_{1,c} \\ s'_{2,c} \\ s'_{3,c} \end{bmatrix} = \begin{bmatrix} 2 & 3 & 1 & 1 \\ 1 & 2 & 3 & 1 \\ 1 & 1 & 2 & 3 \\ 3 & 1 & 1 & 2 \end{bmatrix} \times \begin{bmatrix} s_{0,c} \\ s_{1,c} \\ s_{2,c} \\ s_{3,c} \end{bmatrix}$$

Mục đích: Khiến cho mọi byte ở đầu ra của cột phụ thuộc vào tất cả 4 byte đầu vào của cột đó.

+ AddRoundKey: XOR ma trận hiện tại với khóa vòng (được sinh ra từ khóa gốc).
Ma trận State lúc này (sau khi đã bị xáo trộn, dịch hàng, trộn cột) được đem đi XOR (Exclusive OR) với khóa vòng (Round Key) tương ứng của vòng đó.

- Vòng cuối cùng (Vòng 10): Thực hiện SubBytes, ShiftRows, và AddRoundKey (bỏ qua bước MixColumns).
Vòng cuối (vòng 10, 12 hoặc 14) diễn ra y hệt các vòng chính nhưng bỏ qua bước MixColumns. Trình tự của vòng cuối là: SubBytes -> ShiftRows -> AddRoundKey.

Lý do bỏ MixColumns ở vòng cuối là để giữ cho cấu trúc toán học của quá trình mã hóa và giải mã có tính đối xứng. Nếu giữ MixColumns, quá trình giải mã sẽ phức tạp hơn mà không tăng thêm mức độ bảo mật.

Sau bước AddRoundKey cuối cùng, ma trận State chính là bản mã hóa (Ciphertext) 128-bit.

- Quy trình giải mã: Sử dụng các hàm đảo ngược (InvSubBytes, InvShiftRows, InvMixColumns) và áp dụng các khóa vòng theo thứ tự từ cuối lên đầu.

Khởi tạo: Nạp dữ liệu vào Ma trận State

Đầu tiên, máy tính chuyển chuỗi ký tự thành mã Hex (hệ thập lục phân) theo bảng mã ASCII
Dữ liệu được xếp vào một ma trận 4x4 (gọi là ma trận State) theo thứ tự từ trên xuống dưới, từ trái qua phải (từng cột một)
$$\begin{bmatrix} 4D & 4F & 45 & 32 \\ 
41 & 41 & 53 & 33 \\ 
5F & 5F & 5F & 34 \\ 
48 & 41 & 31 & 35 \end{bmatrix}$$

Trước khi bắt đầu các vòng lặp, ma trận này được XOR với Khóa bí mật (AddRoundKey vòng 0). 

SubBytes (Thay chữ)
AES sử dụng một bảng tra cứu cố định có tên là S-Box. Bảng này định nghĩa sẵn rằng mọi byte đầu vào sẽ bị thay bằng một byte hoàn toàn khác.

Ví dụ với ký tự đầu tiên là 4D (nằm ở hàng 1, cột 1 của ma trận):

AES tách 4D ra: 4 là chỉ số hàng, D là chỉ số cột trong bảng S-Box.

Giao điểm của hàng 4 và cột D trong bảng S-Box là giá trị E3.

Vậy 4D bị đổi thành E3.

Thuật toán làm tương tự cho cả 16 byte. Giả sử sau khi tra bảng xong, ta thu được ma trận mới:
