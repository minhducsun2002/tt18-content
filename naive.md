# String Matching

<!-- naive.md -->
<!-- Written by Nguyen Tuan Dung <@dungwinux> -->

## The Naïve Way

### Định nghĩa

Để bắt đầu cho String-Matching, chúng ta sẽ khởi động với phương thức cơ bản
nhất, gọi là Naïve String Matching.

Nói ngắn gọn, đây là một thuật rất đơn giản và kém hiệu quả. Nó sẽ di chuyển
qua từng vị trí một, so sánh giữa xâu trong văn bản và xâu cần tìm.

Chẳng hạn, cho `target = "qwertyuiop"`  và `pattern = "rtyui"`. Naive sẽ lấy
xâu con từ vị trí `target[0]`, độ dài bằng độ dài `pattern`, đem so với `pattern`
từng kí tự một. Rồi Naive lấy xâu con từ vị trí `target[1]`, độ dài bằng độ dài
`pattern` tiếp tục đem so với `pattern` và cứ như thế cho đến cuối xâu.

Bạn đọc có thể gập sách lại cài thử. Nếu bạn quá bí, thì sau đây là code mẫu:

```cpp
#include <iostream>
#include <string>

int naive_search(std::string target, std::string pattern)
{
    for (int i = 0; i < target.size() - pattern.size(); ++i)
        if (target.substr(i, pattern.size()) == pattern)
            /* Return: Success */
            return i;
    /* Return: Not Found */
    return -1;
}
int main()
{
    std::string target, pattern;
    std::cout << naive_search(target, pattern);
    return 0;
}
```
### Giải thích

#### Tại sao phương pháp này lại đúng?

Rất đơn giản, bởi lẽ 2 xâu khớp nhau luôn có cùng độ dài. Điều đó dẫn đến việc
so sánh toàn bộ các chuỗi con có thể có trong văn bản với độ dài đó. Gọi độ dài
văn bản (có thể chứa mẫu) là `n`, độ dài mẫu là `m`. Ta dễ dàng thấy được có 
_n - m + 1_ chuỗi con với độ dài là `m` -> so sánh _n - m + 1_ chuỗi con.

Phương pháp này có độ phức tạp tính toán là O(_(n - m + 1) * m_), vốn không tối
ưu, đặc biệt khi nhiều phép so sánh là không cần thiết.

### Kết

Pros:
- Không có tiền xử lí (Preprocessing), không sử dụng bộ nhớ
- **Cực kì** dễ hiểu, dễ cài

Cons:
- Thời gian chạy lâu: O(_mn_) do thực hiện nhiều bước không cần thiết
- Kém hiệu quả -> không có ứng dụng