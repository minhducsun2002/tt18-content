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

Trên lý thuyết, thuật toán Rabin-Karp sẽ hash xâu cần tìm (pattern), rồi hash
từng đoạn một trong xâu hướng đến (target) để so sánh với hash của pattern. 
Để có thể tối ưu cho Rabin-Karp, chúng ta sử dụng Rolling Hash, cho phép
chúng ta hash toàn bộ đoạn con của xâu trong O(_n_).

Pros: 
- So sánh số với số, nhanh hơn khi so xâu với xâu.
- Có thể cải tiến để tìm nhiều xâu cùng một lúc.

Cons:
- Rabin-Karp thiên về random, đòi hỏi người dùng phải có chút common sense để
optimize thuật toán.
- Tuy độ phức tạp thấp nhưng Rabin-Karp vẫn chưa phải là thuật nhanh nhất
(Booyer-Moore, KMP, ...)
