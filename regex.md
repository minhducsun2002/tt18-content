# String Matching

<!-- regex.md -->
<!-- Written by Nguyen Tuan Dung <@dungwinux> -->

## Biểu thức chính quy

### Định nghĩa

Biểu thức chính quy (_Regular Expression_ hay _regex_) là một dãy liên tiếp các
ký tự định nghĩa một _mẫu tìm kiếm_.

### Cài đặt

Trong C++ 11 trở lên, STL có sẵn thư viện `<regex>` để hỗ trợ việc sử dụng biểu
thức chính quy. Sau đây là một chương trình cơ bản tìm kiếm trong xâu bằng biểu
thức chính quy:

```cpp
#include <iostream>
#include <string>
#include <regex>
/**
 * @param: content: xâu được tìm kiếm
 * @param: pattern: biểu thức chính quy
 */
int main()
{
    std::string content, pattern;

    /* Input */
    std::cin >> content >> pattern;

    /* Chuyển sang class regex */
    std::regex expr(pattern);

    /* Tìm kiếm trong xâu */
    if (std::regex_search(content, expr))
        std::cout << "Text contains string that match pattern \n";
    else
        std::cout << "Text doesn't contain string that match pattern \n";
    return 0;
}
```

Bài viết này sẽ diễn giải cụ thể các chức năng cơ bản nhất về biểu thức chính
quy. Để đạt hiệu quả cao, bạn đọc có thể mở [regex101.com](https://regex101.com)
để có thể hiểu đựoc các ví dụ một cách trực quan. Bạn đọc có thể vần dụng Google
để biết thêm về các chức năng khác.

#### Tìm trực tiếp

Rất đơn giản, để tìm một xâu cụ thể, bạn chỉ cần đặt biểu thức chính quy là xâu,
và thế là xong.

```regex
hello
```

#### Tìm xâu biết trước tiền tố hoặc hậu tố

Để tìm xâu là bắt đầu của một xâu khác, hãy sử dụng `^`

```
^hello
```

Biểu thức trên sẽ tìm tất cả các xâu bắt đâu là `hello`

Để tìm xâu là kết thúc của một xâu khác, hãy sử dụng `^`

```
hello$
```

Biểu thức trên sẽ tìm tất cả các xâu có kết thúc là `hello`

#### Phép định lượng

Đã có biểu thức thì có phép tính. Trong regex có phép đặc biệt, gọi là phép định
luợng. Trong trường hợp tìm kiếm xâu có những ký tự chưa biết, chúng ta phải làm
thế nào? Sau đây là một số biểu thức giúp xử lý vấn đề này.

|  Biểu thức   | Hành động                                                    |
| :----------: | :----------------------------------------------------------- |
|    `zxc*`    | Tìm xâu có chứa `zx` và theo sau là **0 hoặc nhiều ký tự c** |
|    `zxc+`    | Tìm xâu có chứa `zx` và theo sau là **1 hoặc nhiều ký tự c** |
|    `zxc?`    | Tìm xâu có chứa `zx` và theo sau là **0 hoặc 1 ký tự c**     |
|   `qwe{3}`   | Tìm xâu có chứa `qw` và theo sau là **3 ký tự e**            |
|  `qwe{2,}`   | Tìm xâu có chứa `qw` và theo sau là **2 hoặc >= 2 ký tự e**  |
| `a(bc){4,7}` | Tìm xâu có chứa `a` và theo sau là **4 đến 7 lần cụm `bc`**  |

> **Chú ý**: `*`, `+` và `{}` là các phép tham lam, tức chúng sẽ tìm xâu dài nhất
> có thể mà vẫn thỏa mãn biểu thức chính quy.

#### Biểu thức `[]`

Đây là một biểu thức đặc biệt, dùng để định nghĩa khoảng cho một hoặc nhiều ký
tự, sau đây là ví dụ:

|   Biểu thức   | Hành động                                          |
| :-----------: | :------------------------------------------------- |
|    `[abc]`    | Tìm xâu có chứa hoặc a, hoặc b, hoặc c             |
|    `[a-c]`    | Tuơng tự                                           |
| `[a-fA-F0-9]` | Tìm xâu thể hiện số thập lục phân                  |
|   `[0-9]%`    | Tìm xâu có chữ số trước ký thự `%`                 |
|  `[^a-zA-Z]`  | Tìm xâu không có chứa ký tự chữ cái 'A-Z' và 'a-z' |

### Ứng dụng

Biểu thức chính quy có thể đựoc tìm thấy ở nhiều nơi, như trong tính năng _Find_
và _Find and Replace_ trong các trình sửa văn bản. Trong GNU/Linux có chương
trình `find` và `grep` nhận input là regex để tìm kiếm.

### Kết luận

Xem thêm các hàm và thuật toán trong thư viện `<regex>` tại
https://en.cppreference.com/w/cpp/regex.

Pros:
- Tìm kiếm chuyên sâu và thông minh, đăc biệt với những pattern phức tạp.
- Nhiều ngôn ngữ như C++, Python, JavaScript, Crystal, ... có thư viện hỗ trợ sẵn,
lập trình viên  chỉ việc gọi ra.

Cons:
- Khó đọc và viết, -> khó sửa và gỡ lỗi code.
- Ảnh hưởng ít nhiều tới hiệu năng và làm chậm chuơng trình.

### Tham khảo

1. https://en.wikipedia.org/wiki/Regular_expression
2. https://en.cppreference.com/w/cpp/regex
3. https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285
4. https://github.com/42tm/wfind