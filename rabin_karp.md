# String Matching

<!-- rabin_karp.md -->
<!-- Written by Nguyen Tuan Dung <@dungwinux> -->

## Thuật toán Rabin-Karp

### Định nghĩa

Đây là thuật toán tình kiếm xâu đựoc tạo ra bởi Richard M. Karp và Michael O.
Rabin vào năm 1987. Thuật toán này sử dụng hàm hash để tìm bất cứ xâu con nào có
trong xâu.

Với xâu độ dài là n, và xâu con độ dài m, trường hợp trung bình và tốt nhất là
O(_n + m_), còn trường hợp xấu nhất là O(_nm_).

#### Implementation

[https://github.com/dungwinux/string-matching/blob/master/RabinKarp.cpp](https://github.com/dungwinux/string-matching/blob/master/RabinKarp.cpp)

Như code ở trên, dòng 4, 10, 13 cần O(_m_). Tuy nhiên, dòng hai chỉ chạy một
lần, và dòng 6 chỉ chạy khi giá trị hash bằng nhau, thường chỉ chạy vài lần.
Giờ chúng ta cần quan tâm tới hàm hash.

Để có thể tối ưu cho Rabin-Karp, chúng ta sử dụng Rolling Hash, cho phép
chúng ta hash toàn bộ đoạn con của xâu trong O(_n_)
