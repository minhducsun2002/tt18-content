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

```cpp
std::size_t RabinKarp(std::string target, std::string pattern)
{
    std::size_t count = 0;
    std::size_t loop_size = target.length() - pattern.length() + 1;

    std::string hashPattern = hash(pattern);

    /* Chạy vòng lặp qua xâu */
    for (std::size_t iter = 0; iter < loop_size; ++iter)
    {
        /* So sánh hash của 2 xâu con */
        std::string hashSubTarget = target.substr(iter, pattern.length());
        if (hash(hashSubTarget) == hashPattern)
            /* So sánh hai xâu */
            if (hashSubTarget == pattern)
                ++count;
    }

    return count;
}
```

Xem implementation đầy đủ trên GitHub: [https://github.com/dungwinux/string-matching/blob/master/RabinKarp.cpp](https://github.com/dungwinux/string-matching/blob/master/RabinKarp.cpp)

Như code ở trên, dòng 4, 10, 13 cần O(_m_). Tuy nhiên, dòng hai chỉ chạy một
lần, và dòng 6 chỉ chạy khi giá trị hash bằng nhau, thường chỉ chạy vài lần.
Giờ chúng ta cần quan tâm tới hàm hash.

Cách tích hợp hàm hash đơn giản nhất trong Rabin-Karp là
```cpp
int Hash(std::string str)
{
    /* Hằng số */
    int cnst = 256;
    /* Modulo */
    int mod = 1e9 + 7;
    /* Biến lưu kết quả */
    int sum = 0;
    
    // Lặp từ cuối tới đầu xâu
    for (auto iter = str.rbegin(); iter != str.rend(); ++iter)
        sum = (sum * cnst + str) % mod;
    return sum;
}
```

Tuy nhiên, sử dụng phương thức trên đây sẽ ăn may 50-50 vì nó phần lớn phụ
thuộc vào biến `cnst` và `mod` mà chúng ta chọn bởi nó có khả năng dẫn đến Hash
Collision, tức hai dữ liệu khác nhau nhưng có cùng một hash.
