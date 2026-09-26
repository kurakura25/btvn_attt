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

<img width="477" height="166" alt="image" src="https://github.com/user-attachments/assets/248d693e-cba9-448f-948c-03917220a2eb" />


Mục đích: Khiến cho mọi byte ở đầu ra của cột phụ thuộc vào tất cả 4 byte đầu vào của cột đó.

+ AddRoundKey: XOR ma trận hiện tại với khóa vòng (được sinh ra từ khóa gốc).
Ma trận State lúc này (sau khi đã bị xáo trộn, dịch hàng, trộn cột) được đem đi XOR (Exclusive OR) với khóa vòng (Round Key) tương ứng của vòng đó.

- Vòng cuối cùng (Vòng 10): Thực hiện SubBytes, ShiftRows, và AddRoundKey (bỏ qua bước MixColumns).
Vòng cuối (vòng 10, 12 hoặc 14) diễn ra y hệt các vòng chính nhưng bỏ qua bước MixColumns. Trình tự của vòng cuối là: SubBytes -> ShiftRows -> AddRoundKey.

Lý do bỏ MixColumns ở vòng cuối là để giữ cho cấu trúc toán học của quá trình mã hóa và giải mã có tính đối xứng. Nếu giữ MixColumns, quá trình giải mã sẽ phức tạp hơn mà không tăng thêm mức độ bảo mật.

Sau bước AddRoundKey cuối cùng, ma trận State chính là bản mã hóa (Ciphertext) 128-bit.

- Quy trình giải mã: Sử dụng các hàm đảo ngược (InvSubBytes, InvShiftRows, InvMixColumns) và áp dụng các khóa vòng theo thứ tự từ cuối lên đầu.

Mô tả thuật toán chi tiết:

Khởi tạo: Nạp dữ liệu vào Ma trận State

Đầu tiên, máy tính chuyển chuỗi ký tự thành mã Hex (hệ thập lục phân) theo bảng mã ASCII
Dữ liệu được xếp vào một ma trận 4x4 (gọi là ma trận State) theo thứ tự từ trên xuống dưới, từ trái qua phải (từng cột một)

<img width="322" height="175" alt="image" src="https://github.com/user-attachments/assets/3883fb56-43ef-4123-bacd-cd69a3c1d86a" />


Trước khi bắt đầu các vòng lặp, ma trận này được XOR với Khóa bí mật (AddRoundKey vòng 0). 

SubBytes (Thay chữ)
AES sử dụng một bảng tra cứu cố định có tên là S-Box. Bảng này định nghĩa sẵn rằng mọi byte đầu vào sẽ bị thay bằng một byte hoàn toàn khác.

Ví dụ với ký tự đầu tiên là 4D (nằm ở hàng 1, cột 1 của ma trận):

AES tách 4D ra: 4 là chỉ số hàng, D là chỉ số cột trong bảng S-Box.

Giao điểm của hàng 4 và cột D trong bảng S-Box là giá trị E3.

Vậy 4D bị đổi thành E3.

Thuật toán làm tương tự cho cả 16 byte. Giả sử sau khi tra bảng xong, ta thu được ma trận mới:

<img width="361" height="192" alt="image" src="https://github.com/user-attachments/assets/28f8c0da-78b7-4d0b-892f-5cacdbe99577" />

Bước 2: ShiftRows (Dịch hàng)
Để dữ liệu không bị cô lập theo từng cột, AES đẩy lệch các hàng sang trái. Các byte bị đẩy ra khỏi ma trận bên trái sẽ vòng lại chui vào phía bên phải.

Hàng 0: Giữ nguyên (Dịch 0 bước).

Hàng 1: Dịch trái 1 bước (Byte 83 đầu tiên bị đẩy xuống cuối hàng).

Hàng 2: Dịch trái 2 bước.

Hàng 3: Dịch trái 3 bước (Tương đương dịch phải 1 bước).

Sự biến đổi diễn ra như sau:

<img width="692" height="177" alt="image" src="https://github.com/user-attachments/assets/6e972996-4ac0-4dd1-8eb5-64d480c27ae5" />

2. tìm hiểu về thuật toán mã hoá bất đối xứng RSA
   nguyên lý sinh cặp khoá bí mật, công khai

Bước 3: MixColumns (Trộn cột)
Ở bước này, AES lấy từng cột 4-byte riêng biệt và thực hiện phép nhân ma trận để trộn chúng lại với nhau.

Xét cột đầu tiên của ma trận hiện tại: [E3, 83, CF, 96].

Thuật toán lấy cột này nhân với một ma trận tiêu chuẩn trong Toán học trường Galois:
<img width="408" height="187" alt="image" src="https://github.com/user-attachments/assets/63a68e28-5857-479e-8d76-fccecebb0b2a" />

Sau phép toán phức tạp này, cột E3, 83, CF, 96 biến thành 5A, 1F, 67, B5. Điều đặc biệt của phép MixColumns là chỉ cần 1 byte trong cột ban đầu thay đổi (ví dụ E3 đổi thành E4), thì cả 4 byte ở cột kết quả đều sẽ thay đổi hoàn toàn. Điều này tạo ra hiệu ứng "hiệu ứng cánh bướm" (Avalanche effect) cực kỳ mạnh.

Bước 4: AddRoundKey (Thêm khóa vòng)
Ma trận State vừa trải qua 3 bước nhào lộn sẽ được trộn thêm một lần nữa với Khóa vòng 1 (Round Key 1 - được sinh ra từ khóa bí mật ban đầu). Phép trộn sử dụng

toán tử logic XOR (Exclusive OR) từng bit một.

Sau bước này, Vòng 1 kết thúc. Thuật toán lại lấy ma trận kết quả ném vào Vòng 2, làm y hệt các bước trên, và lặp lại liên tục 10 lần (với AES-128). Sau vòng cuối cùng, ma trận thu được chính là chuỗi ký tự mã hóa vô nghĩa mà hacker nhìn thấy.


### 2. Tìm hiểu về thuật toán mã hoá bất đối xứng RSA nguyên lý sinh cặp khoá bí mật, công khai

RSA (Rivest–Shamir–Adleman) là một trong những hệ thống mã hóa bất đối xứng đầu tiên và được sử dụng rộng rãi nhất hiện nay để truyền dữ liệu an toàn. 

Nguyên lý hoạt động của RSA dựa trên sự bất đối xứng về mặt toán học: việc nhân hai số nguyên tố lớn với nhau thì rất dễ, nhưng việc phân tích tích số của chúng ngược lại thành hai số nguyên tố ban đầu lại cực kỳ khó (bài toán phân tích ra thừa số nguyên tố).

Trong mã hóa bất đối xứng, mỗi bên sẽ có một cặp khóa: Khóa công khai (Public Key) dùng để mã hóa dữ liệu và Khóa bí mật (Private Key) dùng để giải mã.

Nguyên lý sinh cặp khóa RSA


Quy trình tạo ra cặp khóa công khai và bí mật trải qua 5 bước toán học chuẩn như sau:

Bước 1: Chọn hai số nguyên tố lớn ($p$ và $q$)

Hệ thống sẽ chọn ngẫu nhiên hai số nguyên tố rất lớn (trong thực tế, các số này thường dài từ 1024 đến 2048 bit).

Điều kiện: $p \neq q$ và chúng phải được giữ bí mật tuyệt đối.

Bước 2: Tính module ($n$)

Tính tích của hai số nguyên tố vừa chọn:
$$n = p \times q$$

Số $n$ này sẽ được sử dụng làm module cho cả khóa công khai và khóa bí mật. 

Độ dài của $n$ (tính bằng bit) chính là "độ dài khóa" (ví dụ: khóa RSA 2048-bit nghĩa là $n$ dài 2048 bit). $n$ được công bố công khai.

Bước 3: Tính hàm Phi Euler 

Tính số lượng các số nguyên tố cùng nhau với $n$ (nhỏ hơn $n$). 

Dựa vào tính chất của số nguyên tố, hàm này được tính bằng:

$$\phi(n) = (p - 1) \times (q - 1)$$

Giá trị $\phi(n)$ phải được giữ bí mật để phục vụ cho việc tạo khóa bí mật ở Bước 5.

Bước 4: Chọn số mũ công khai ($e$)

Chọn một số nguyên $e$ (public exponent) sao cho:

$1 < e < \phi(n)$

 $e$  và $\phi(n)$ là hai số nguyên tố cùng nhau (tức là ước chung lớn nhất 

$\text{ƯCLN}(e, \phi(n)) = 1$).

Số $e$ này thường được chọn là các số nguyên tố Fermat để tối ưu tốc độ mã hóa (giá trị phổ biến nhất trong thực tế là $65537$).

Bước 5: Tính số mũ bí mật ($d$)

Tính số $d$ (private exponent) sao cho nó là nghịch đảo modulo của $e$ theo module $\phi(n)$. 





Nói cách khác:

$$d \times e \equiv 1 \pmod{\phi(n)}$$

Điều này có nghĩa là khi chia 

$(d \times e)$ cho $\phi(n)$

số dư phải là 1. Người ta thường dùng Thuật toán Euclid mở rộng để tìm ra $d$.

Sau 5 bước trên, chúng ta thu được cặp khóa:

Khóa công khai (Public Key): Cặp số $(n, e)$.

Bạn gửi cặp số này cho bất kỳ ai để họ mã hóa dữ liệu gửi cho bạn.

Khóa bí mật (Private Key): Cặp số $(n, d)$. Bạn phải giữ kín số $d$. 

Các thông số $p, q$ và $\phi(n)$ lúc này có thể bị xóa bỏ hoặc cất giấu, nhưng không bao giờ được tiết lộ.

Ví dụ minh họa

Để dễ hình dung, hãy xem quy trình với các con số nhỏ:

Chọn $p, q$: Chọn $p = 61$ và $q = 53$.

Tính $n$: $n = 61 \times 53 = 3233$.

Tính $\phi(n)$: $\phi(n) = (61 - 1) \times (53 - 1) = 60 \times 52 = 3120$.

Chọn $e$: Chọn một số nguyên tố cùng nhau với $3120$. 

Chọn $e = 17$ (vì $\text{ƯCLN}(17, 3120) = 1$).

Tính $d$: Tìm $d$ sao cho $17 \times d \equiv 1 \pmod{3120}$. 

Bằng thuật toán Euclid mở rộng, ta tìm được $d = 2753$. (Thử lại: $17 \times 2753 = 46801$, và $46801 = 15 \times 3120 + 1$).

Kết quả:

Khóa công khai: $(n=3233, e=17)$

Khóa bí mật: $(n=3233, d=2753)$

Nếu một tin tặc chặn được khóa công khai $(3233, 17)$, họ biết $n = 3233$. Để tìm được khóa bí mật $d$, họ phải tính được $\phi(n)$, nghĩa là họ buộc phải phân tích số $3233$ ngược lại thành $61 \times 53$. Với số $3233$, việc này mất chưa tới 1 giây. Nhưng với một số $n$ có kích thước 2048 bit (khoảng 617 chữ số thập phân), các siêu máy tính hiện đại nhất thế giới cũng phải mất hàng triệu năm để phân tích ra $p$ và $q$.

### 3. Trình bày các mô hình hình áp dụng thuật toán RSA xác thực người gửi, xác thực người nhận, so sánh thời gian mã hoá/giải mã của RSA với AES và đưa ra các dùng kết hợp sức mạnh của RSA và AES.

RSA (Rivest–Shamir–Adleman) là một thuật toán mật mã khóa công khai (Public Key Cryptography), sử dụng một cặp khóa gồm:

- **Public Key (khóa công khai):** có thể công khai cho mọi người.
- **Private Key (khóa riêng):** phải được bảo mật và chỉ chủ sở hữu được sử dụng.

RSA có thể được sử dụng cho nhiều mục đích:

1. Mã hóa và giải mã dữ liệu.

2. Xác thực người gửi.

3. Xác thực tính toàn vẹn của dữ liệu thông qua chữ ký số.

4. Bảo vệ hoặc trao đổi khóa phiên.

5. Kết hợp với AES để xây dựng hệ thống mã hóa lai.

#### Nguyên lý khóa RSA

Mỗi người sử dụng RSA sẽ có một cặp khóa:


                    RSA KEY PAIR
                         │
              ┌──────────┴──────────┐
              │                     │
        Public Key             Private Key
      (Khóa công khai)        (Khóa riêng)
              │                     │
       Có thể công khai        Phải bảo mật

Giả sử hệ thống có hai bên:

Alice: người gửi.
Bob: người nhận.

Mỗi bên có một cặp khóa:

Alice:
    Public Key_Alice
    Private Key_Alice

Bob:
    Public Key_Bob
    Private Key_Bob

Mô hình RSA xác thực người gửi

Mục đích

Mô hình này được sử dụng khi Bob cần xác định:

Thông điệp có thực sự được gửi bởi Alice hay không?

RSA được sử dụng để tạo chữ ký số (Digital Signature).

Alice sử dụng Private Key của Alice để ký.
Bob sử dụng Public Key của Alice để xác minh chữ ký.

Mô hình:

                     ALICE
                  Người gửi
                      │
                      │ Message
                      ▼
                 Hash(Message)
                      │
                      ▼
          ┌────────────────────────┐
          │  Private Key - Alice   │
          └────────────────────────┘
                      │
                      ▼
                Digital Signature
                      │
                      │
          ┌───────────┴───────────┐
          │                       │
          ▼                       ▼
       Message              Signature
          │                       │
          └───────────┬───────────┘
                      │
                      ▼
                     BOB
                 Người nhận
                      │
             ┌────────┴────────┐
             │                 │
             ▼                 ▼
       Hash(Message)     Verify Signature
                               │
                               ▼
                     Public Key - Alice
                               │
                               ▼
                    ┌──────────┴──────────┐
                    │                     │
                  Hợp lệ               Không hợp lệ
                    │                     │
                    ▼                     ▼
             Alice đã ký          Chữ ký sai hoặc
             thông điệp           dữ liệu bị thay đổi

Các bước thực hiện

Bước 1: Alice tạo thông điệp

M = "Chuyen 1000000 VND cho Bob"

Bước 2: Alice tính giá trị Hash
H = Hash(M)

Thông thường hệ thống sẽ sử dụng một hàm băm như SHA-256:

Message
   │
   ▼
 SHA-256
   │
   ▼
 Hash

Bước 3: Alice tạo chữ ký số

Alice sử dụng Private Key của mình để ký giá trị Hash:

Signature = RSA_Sign(
    PrivateKey_Alice,
    Hash(Message)
)
Bước 4: Alice gửi dữ liệu cho Bob

Alice gửi:

Message + Signature

Bước 5: Bob xác minh chữ ký

Bob sử dụng:

PublicKey_Alice

để kiểm tra chữ ký.

Bob đồng thời tính lại:

Hash(Message)

Sau đó kiểm tra:

Hash tính lại
      =
Hash được xác minh từ chữ ký?

Nếu đúng:

✓ Chữ ký hợp lệ

✓ Xác nhận người sở hữu Private Key_Alice đã ký

✓ Dữ liệu chưa bị thay đổi

Nếu sai:

✗ Chữ ký không hợp lệ

✗ Có thể dữ liệu đã bị thay đổi

✗ Hoặc chữ ký không được tạo bởi khóa tương ứng

Mô hình RSA xác thực người nhận

Mục đích

Mô hình này tập trung vào việc đảm bảo:

Chỉ Bob mới có thể giải mã và đọc được nội dung mà Alice gửi.

Alice sử dụng Public Key của Bob để mã hóa.

Bob sử dụng Private Key của Bob để giải mã.

Mô hình hoạt động

                    ALICE
                 Người gửi
                     │
                     │ Message M
                     ▼
          ┌────────────────────────┐
          │    Public Key - Bob    │
          └────────────────────────┘
                     │
                     ▼
               RSA Encryption
                     │
                     ▼
                Ciphertext
                     │
                     │ Gửi qua mạng
                     ▼
                    BOB
                Người nhận
                     │
                     ▼
          ┌────────────────────────┐
          │   Private Key - Bob   │
          └────────────────────────┘
                     │
                     ▼
               RSA Decryption
                     │
                     ▼
                  Message M

Các bước thực hiện

Bước 1: Alice tạo thông điệp

M = "Thong tin bi mat"

Bước 2: Alice lấy Public Key của Bob

PublicKey_Bob

Bước 3: Alice mã hóa

C = RSA_Encrypt(
    PublicKey_Bob,
    M
)

Kết quả:

Message
   │
   ▼
RSA + Public Key Bob
   │
   ▼
Ciphertext
Bước 4: Alice gửi Ciphertext cho Bob
Alice
  │
  │ Ciphertext
  ▼
Internet
  │
  ▼
Bob

Nếu kẻ tấn công lấy được Ciphertext thì không thể dễ dàng đọc được nội dung nếu không có Private Key tương ứng của Bob.

Bước 5: Bob giải mã

Bob sử dụng Private Key:

PrivateKey_Bob

để giải mã:

M = RSA_Decrypt(
    PrivateKey_Bob,
    C
)

Kết quả:

Ciphertext
     │
     ▼
Private Key Bob
     │
     ▼
Original Message

 Ý nghĩa bảo mật

Mô hình này cung cấp tính bí mật (Confidentiality).

Có thể hiểu đơn giản:

Alice
  │
  │ Mã hóa bằng Public Key của Bob
  ▼
Ciphertext
  │
  │ Chỉ Private Key của Bob có thể giải mã
  ▼
Bob


Mô hình RSA xác thực cả người gửi và người nhận

Mục đích

Đây là mô hình kết hợp hai yêu cầu:

Xác thực người gửi.

Đảm bảo chỉ người nhận có thể đọc dữ liệu.

Alice muốn Bob biết rằng:

✓ Dữ liệu thực sự được Alice gửi.

✓ Dữ liệu không bị thay đổi.

✓ Chỉ Bob có thể đọc nội dung.

Mô hình sử dụng:

Private Key_Alice

Public Key_Alice

Public Key_Bob

Private Key_Bob
Mô hình hoạt động

                         ALICE
                      Người gửi
                          │
                          │
                     Message M
                          │
                          ▼
                    Hash(Message)
                          │
                          ▼
              ┌──────────────────────┐
              │ Private Key - Alice  │
              └──────────────────────┘
                          │
                          ▼
                    Digital Signature
                          │
                          ▼
                  Message + Signature
                          │
                          ▼
              ┌──────────────────────┐
              │   Public Key - Bob   │
              └──────────────────────┘
                          │
                          ▼
                    RSA Encryption
                          │
                          ▼
                  Encrypted Package
                          │
                          │
                     Internet
                          │
                          ▼
                          BOB
                     Người nhận
                          │
                          ▼
              ┌──────────────────────┐
              │  Private Key - Bob   │
              └──────────────────────┘
                          │
                          ▼
                    RSA Decryption
                          │
                          ▼
                  Message + Signature
                          │
              ┌───────────┴───────────┐
              │                       │
              ▼                       ▼
       Hash(Message)           Verify Signature
                                      │
                                      ▼
                             Public Key - Alice
                                      │
                                      ▼
                              Signature hợp lệ

Các bước thực hiện

Bước 1: Alice tạo Hash

H = Hash(Message)

Bước 2: Alice ký bằng Private Key

Signature = RSA_Sign(
    PrivateKey_Alice,
    H
)

Bước 3: Tạo gói dữ liệu

Package = Message + Signature

Bước 4: Mã hóa bằng Public Key của Bob

Ciphertext = RSA_Encrypt(
    PublicKey_Bob,
    Package
)

Bước 5: Gửi Ciphertext

Alice
  │
  │ Ciphertext
  ▼
Network
  │
  ▼
Bob

Bước 6: Bob giải mã

Package = RSA_Decrypt(
    PrivateKey_Bob,
    Ciphertext
)

Bob nhận được:

Message + Signature

Bước 7: Bob xác minh chữ ký

Bob sử dụng:

PublicKey_Alice

để xác minh:

Verify(
    PublicKey_Alice,
    Message,
    Signature
)

Nếu hợp lệ:

✓ Xác thực được chữ ký của Alice

✓ Dữ liệu chưa bị thay đổi

✓ Nội dung đã được bảo vệ khi truyền đến Bob

So sánh thời gian mã hóa và giải mã RSA với AES

Tổng quan

RSA và AES là hai loại thuật toán mật mã khác nhau.

<img width="747" height="621" alt="image" src="https://github.com/user-attachments/assets/e3cc8e29-e010-49b1-a32f-8171c620c691" />


So sánh tốc độ

Khi xử lý cùng một lượng dữ liệu, AES thường nhanh hơn RSA rất nhiều.

Minh họa:

Thời gian xử lý
│
│                         RSA
│                         ███████████████████
│
│
│
│     AES
│     █
│     █
│     █
│
└──────────────────────────────────►
             Dữ liệu

AES được thiết kế để mã hóa dữ liệu với tốc độ cao.

RSA sử dụng các phép toán số học trên các số nguyên rất lớn nên chi phí tính toán cao hơn.

Vì sao RSA chậm hơn AES?

RSA sử dụng các phép toán trên số nguyên lớn.

Về mặt khái niệm, RSA thực hiện các phép toán dạng:

C = M^e mod n

và:

M = C^d mod n

Trong đó n, e, d có thể là các số nguyên rất lớn.

Các kích thước khóa RSA thường gặp:

RSA-2048

RSA-3072

RSA-4096

Trong khi đó AES được thiết kế để xử lý dữ liệu theo các khối 128 bit:

AES-128

AES-192

AES-256

AES có thể xử lý dữ liệu rất nhanh và trên nhiều CPU còn được hỗ trợ tăng tốc bằng phần cứng.

Ví dụ về dữ liệu lớn

Giả sử cần mã hóa:

10 KB

100 KB

1 MB

10 MB

100 MB

1 GB

AES có thể trực tiếp xử lý các dữ liệu này:

File
 │
 ▼
AES
 │
 ▼
Encrypted File

Trong khi RSA không được thiết kế để mã hóa trực tiếp các file lớn.

Do đó không nên thực hiện:

File 1 GB
   │
   ▼
  RSA
   │
   ▼
Encrypted File

Mà nên sử dụng:

File 1 GB
   │
   ▼
  AES
   │
   ▼
Encrypted File

RSA chỉ đảm nhận một phần nhỏ, chẳng hạn bảo vệ khóa AES.

Thực nghiệm đo thời gian

Để so sánh công bằng, có thể xây dựng chương trình thử nghiệm:

                  CHƯƠNG TRÌNH TEST
                         │
             ┌───────────┴───────────┐
             │                       │
            RSA                     AES
             │                       │
       Mã hóa dữ liệu          Mã hóa dữ liệu
             │                       │
       Đo thời gian             Đo thời gian
             │                       │
       Giải mã dữ liệu          Giải mã dữ liệu
             │                       │
       Đo thời gian             Đo thời gian
             │                       │
             └───────────┬───────────┘
                         │
                         ▼
                   So sánh kết quả

Có thể thử với các kích thước dữ liệu khác nhau:

10 KB

100 KB

1 MB

10 MB

100 MB

Tuy nhiên, đối với RSA nên thực nghiệm trên dữ liệu nhỏ hoặc đo các thao tác RSA phù hợp, vì RSA không được thiết kế để mã hóa file lớn trực tiếp.

Các yếu tố ảnh hưởng đến thời gian

Thời gian mã hóa và giải mã phụ thuộc vào:

CPU

RAM

Hệ điều hành

Ngôn ngữ lập trình

Thư viện mã hóa

Kích thước khóa RSA

Loại AES

Chế độ hoạt động của AES

Kích thước dữ liệu

Khả năng tăng tốc phần cứng

Do đó khi thực nghiệm cần sử dụng cùng:

Máy tính
+
Hệ điều hành
+
Ngôn ngữ
+
Thư viện
+
Kích thước dữ liệu

để kết quả có tính so sánh.

Kết hợp sức mạnh của RSA và AES

RSA và AES có các ưu điểm khác nhau.

RSA có ưu điểm

✓ Sử dụng Public Key và Private Key

✓ Có thể xác thực bằng chữ ký số

✓ Có thể bảo vệ khóa phiên

✓ Không cần gửi Secret Key trực tiếp dưới dạng plaintext

RSA có nhược điểm

✗ Tốc độ chậm hơn AES

✗ Chi phí tính toán cao

✗ Không phù hợp với dữ liệu lớn

AES có ưu điểm

✓ Tốc độ cao

✓ Phù hợp dữ liệu lớn

✓ Phù hợp mã hóa file

✓ Chi phí tính toán thấp hơn RSA

AES có nhược điểm

✗ Hai bên phải có cùng Secret Key

✗ Cần cơ chế an toàn để trao đổi khóa

Vì vậy có thể kết hợp:

RSA → Bảo vệ / trao đổi AES Key

AES → Mã hóa dữ liệu

Mô hình này được gọi là:

Hybrid Encryption – Mã hóa lai


Mô hình Hybrid Encryption RSA + AES

Sơ đồ tổng quát
                         ALICE
                     Người gửi
                         │
                         ▼
                    Dữ liệu gốc
                         │
                         │
                         ▼
              ┌─────────────────────┐
              │ Tạo AES Session Key │
              └─────────────────────┘
                         │
                  ┌──────┴──────┐
                  │             │
                  ▼             ▼
             AES Session      Dữ liệu
                 Key              │
                  │               ▼
                  │          AES Encrypt
                  │               │
                  │               ▼
                  │          Ciphertext
                  │
                  ▼
             RSA Encrypt
                  │
             Public Key Bob
                  │
                  ▼
          Encrypted AES Key
                  │
                  │
          ┌───────┴────────┐
          │  Gửi qua mạng  │
          └───────┬────────┘
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
Encrypted AES Key       Ciphertext
        │                   │
        └─────────┬─────────┘
                  │
                  ▼
                 BOB
             Người nhận
                  │
                  ▼
             RSA Decrypt
             Private Key Bob
                  │
                  ▼
              AES Key
                  │
                  ▼
             AES Decrypt
                  │
                  ▼
              Dữ liệu gốc


Các bước hoạt động của RSA + AES

Bước 1: Alice tạo AES Session Key

Alice tạo một khóa AES ngẫu nhiên:

AES_Key = Random()

Ví dụ:

AES-256 → khóa 256 bit

Khóa này được sử dụng cho phiên giao tiếp hiện tại.

Bước 2: Alice dùng AES để mã hóa dữ liệu

Ciphertext = AES_Encrypt(
    AES_Key,
    Data
)

Ví dụ:

File
 │
 ▼
AES-256
 │
 ▼
Encrypted File

AES đảm nhận việc mã hóa dữ liệu lớn.

Bước 3: Alice dùng RSA để mã hóa AES Key

Alice sử dụng Public Key của Bob:

Encrypted_AES_Key = RSA_Encrypt(
    PublicKey_Bob,
    AES_Key
)

Điểm quan trọng là RSA chỉ mã hóa:

AES Key

chứ không mã hóa toàn bộ file.

Bước 4: Alice gửi dữ liệu

Alice gửi:

Encrypted_AES_Key
+
Ciphertext

Có thể biểu diễn:

┌───────────────────────────────┐
│ Encrypted AES Key             │
├───────────────────────────────┤
│ AES Ciphertext                │
└───────────────────────────────┘
Bước 5: Bob giải mã AES Key

Bob sử dụng Private Key của mình:

AES_Key = RSA_Decrypt(
    PrivateKey_Bob,
    Encrypted_AES_Key
)
Bước 6: Bob giải mã dữ liệu

Bob sử dụng AES Key vừa lấy được:

Data = AES_Decrypt(
    AES_Key,
    Ciphertext
)

Kết quả:

Encrypted File
      │
      ▼
    AES Key
      │
      ▼
Original File

Kết hợp RSA + AES + Chữ ký số

Có thể mở rộng mô hình bằng cách kết hợp:

RSA
 │
 ├── Bảo vệ AES Key
 │
 └── Chữ ký số

AES
 │
 └── Mã hóa dữ liệu

Mô hình:

                         ALICE
                           │
                           ▼
                         DATA
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
        Hash(DATA)                    AES
              │                         │
              ▼                         ▼
      RSA Signature              Ciphertext
      Private Key Alice
              │
              │
              ▼
       Digital Signature

                    AES Session Key
                           │
                           ▼
                   RSA Encryption
                   Public Key Bob
                           │
                           ▼
                  Encrypted AES Key

Alice gửi cho Bob:

Ciphertext
+
Encrypted AES Key
+
Digital Signature

Bob thực hiện:

Encrypted AES Key
        │
        ▼
RSA Decrypt
Private Key Bob
        │
        ▼
AES Session Key
        │
        ▼
AES Decrypt
        │
        ▼
Original Data
        │
        ▼
Verify Signature
        │
        ▼
Public Key Alice

Nếu chữ ký hợp lệ:

✓ Xác thực người gửi

✓ Kiểm tra tính toàn vẹn dữ liệu

✓ Dữ liệu được mã hóa

✓ Chỉ Bob có thể lấy AES Key
