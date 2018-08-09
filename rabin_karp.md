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

### Implementation

Full implementation: https://github.com/dungwinux/string-matching/blob/master/RabinKarp.cpp

> Để thuận tiện, người viết sẽ sử dụng các tên biến đựoc viết trong đoạn code 
> ở trên:
> - `target`: Xâu văn bản, (có thể) có chứa xâu tìm kiếm
> - `pattern`: Xâu cần tìm
> - `n`: Độ dài xâu văn bản
> - `m`: Độ dài xâu cần tìm
> - `cnst = 257`: Hằng số cho trước, có thể hiểu là seed
> - `mod = 1'000'000'007`: Giới hạn của hash
> - `patternHash`: hash của xâu cần tìm
> - `rollingHash`: hash của đoạn con `[i .. (i + m)]` trong xâu văn bản

Khác với các thuật toán khác như KMP, Boyer-Moore chơi trò nhảy qua ký tự, Rabin-Karp "_cẩn thận_" so sánh các xâu con của _target_ với _pattern_ bằng hàm hash như sau:

![Caculating Hash](/img/rkarp-1.svg)

##### Code C++

```cpp
unsigned hash (const std::string &str)
{
    long long sum = 0;
    for (auto &iter : str)
    {
        sum = sum * cnst + iter;
        sum %= mod;
    }
    return sum;
}
```

Độ phức tạp của hàm hash trên là O(_m_) với n là độ dài xâu. Chúng ta cần phải
hash từ đầu đến cuối xâu để so sánh, vậy là mất O(_nm_). 

Bây giờ, chúng ta sẽ có input vào là `target = "awesomeduc"` và `pattern = "duc"`

| `target`  | `a` | `w` | `e` | `s` | `o` | `m` | `e` | `d` | `u` | `c` |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `pattern` | `d` | `u` | `c` |     |     |     |     |     |     |     |

Trước tiên, chúng ta sẽ tiến hành hash xâu `pattern` trước, được giá trị
`6635068`. Chuyển sang xâu `target`, chúng ta sẽ hash ba ký tự đầu (do `pattern`
có 3 ký tự) được giá trị `6437437`. Tất nhiên, `6437437 != 6635068`.

Vì giá trị hash khác nhau, nên ta có thể hiểu giá trị hai xâu khác nhau, ta có
thể tiếp tục so sánh với hash của 3 ký tự tiếp của xâu `target`.

| `target`  | `a` | `w` | `e` | `s` | `o` | `m` | `e` | `d` | `u` | `c` |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `pattern` |     | `d` | `u` | `c` |     |     |     |     |     |     |

Để có thể tối ưu cho Rabin-Karp, chúng ta sử dụng Rolling Hash, cho phép chúng
ta hash toàn bộ đoạn con của xâu trong O(_n_). Thay vì chúng ta hash lại 2 ký tự
đã tính hash từ trước, chúng ta có thể cộng `rollingHash` với ký tự mới thêm vào
rồi trừ đi kí tự đầu tiên.

| `target`                                         | [0] | [1] | [2] | [3] | _hash_      |
| ------------------------------------------------ | --- | --- | --- | --- | ----------- |
| `rollingHash`                                    | `a` | `w` | `e` |     | `6437437`   |
| Thêm `s` (`rollingHash * cnst + s[3]`)           | `a` | `w` | `e` | `s` | `654421417` |
| Bỏ `a` (`rollingHash - s[0] * pow(cnst, m + 1)`) |     | `w` | `e` | `s` | `7885903`   |

Chúng ta làm tiếp tục như vậy cho đến khi đạt được vị trí xâu cần tìm:

| `target`  | `a` | `w` | `e` | `s` | `o` | `m` | `e` | `d` | `u` | `c` |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `pattern` |     | `d` | `u` | `c` |     |     |     |     |     |     |
>`hash(target[1..3]) = 7885903`

| `target`  | `a` | `w` | `e` | `s` | `o` | `m` | `e` | `d` | `u` | `c` |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `pattern` |     |     | `d` | `u` | `c` |     |     |     |     |     |
>`hash(target[2..4]) = 6700615`

| `target`  | `a` | `w` | `e` | `s` | `o` | `m` | `e` | `d` | `u` | `c` |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `pattern` |     |     |     | `d` | `u` | `c` |     |     |     |     |
>`hash(target[3..5]) = 7624271`

| `target`  | `a` | `w` | `e` | `s` | `o` | `m` | `e` | `d` | `u` | `c` |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `pattern` |     |     |     |     | `d` | `u` | `c` |     |     |     |
>`hash(target[4..6]) = 7359553`

| `target`  | `a` | `w` | `e` | `s` | `o` | `m` | `e` | `d` | `u` | `c` |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `pattern` |     |     |     |     |     | `d` | `u` | `c` |     |     |
>`hash(target[5..7]) = 7225398`

| `target`  | `a` | `w` | `e` | `s` | `o` | `m` | `e` | `d` | `u` | `c` |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `pattern` |     |     |     |     |     |     | `d` | `u` | `c` |     |
>`hash(target[6..8]) = 6696766`

| `target`  | `a` | `w` | `e` | `s` | `o` | `m` | `e` | `d` | `u` | `c` |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `pattern` |     |     |     |     |     |     |     | `d` | `u` | `c` |
>`hash(target[7..9]) = 6635068 = patternHash`

=> Tìm thấy kết quả

### Kết

Pros: 
- Truờng hợp tốt: O(_n + m_)
- So sánh số với số, nhanh hơn khi so xâu với xâu.
- Có thể cải tiến để tìm nhiều xâu cùng một lúc.
- Dễ hiểu, không cần động não nhiều.

Cons:
- Rabin-Karp thiên về random, đòi hỏi người dùng phải có chút common sense để
optimize thuật toán.
- Có khả năng xảy ra hash collision, tức hai xâu giá trị khác nhau nhưng cùng
mã hash
    > *Giải pháp*: hash collision có thể được giảm nhẹ bằng cách sử dụng nhiều 
    > hash table cùng một lúc với các hằng số và giá trị modulo khác nhau
- Tuy độ phức tạp thấp nhưng Rabin-Karp vẫn chưa phải là thuật nhanh nhất
(còn có Boyer-Moore, KMP, ...)

### Tham khảo

1. https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm

<img src="./img/rkarp-ref-1.svg" width=100px>

2. http://www-igm.univ-mlv.fr/~lecroq/string/node5.html

<img src="./img/rkarp-ref-2.svg" width=100px>

3. https://github.com/dungwinux/string-matching

### Bài tập
- https://codeforces.com/contest/1016/problem/B
- https://codeforces.com/contest/271/problem/D

...