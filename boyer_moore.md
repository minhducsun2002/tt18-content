# String Matching

## Boyer-Moore và Boyer-Moore-Horspool

### Định nghĩa
Trong khoa học máy tính, **Boyer-Moore** là thuật toán tìm kiếm xâu phổ biến và
hiệu quả. Nó được phát triển bởi _Robert S. Boyer_ và _J Strother Moore_ vào năm
1977 . Thuật toán này sẽ xử lý trước xâu cần tìm (_pattern_) chứ không phải xâu
văn bản (_target_). Vì thế, nó phù hợp với ứng dụng tìm xâu con trong xâu văn bản rất lớn. Trên thực tế, văn bản càng dài thì thuật toán chạy càng nhanh.

Ngoài ra còn có phiên bản đơn giản hóa của **Boyer-Moore** là 
**Boyer-Moore-Horspool**. Nó đuợc xuất bản bởi _Nigel Horspool_ vào năm 1980. Nó
cũng thực hiện thao tác tiền xử lý giống **Boyer-Moore** , nhưng chạy qua xâu văn
bản giống như KMP.

### Cài đặt 

Trong C++ 17 trở lên, STL có sẵn class `std::boyer_moore_searcher` và class
`std::boyer_moore_horspool_searcher` trong thư viện `<functional>`. Ta có thể kết
hợp chúng với `std::search` trong thư viện `<algorithm>` để có được một hàm
String-Matching hoàn chỉnh.

```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <functional>
 
int main()
{
    std::string in = "Lorem ipsum dolor sit amet, consectetur adipiscing elit,"
                     " sed do eiusmod tempor incididunt ut labore et dolore magna aliqua";
    std::string needle = "pisci";
    auto it = std::search(in.begin(), in.end(),
                   std::boyer_moore_searcher(
                       needle.begin(), needle.end()));
    if(it != in.end())
        std::cout << "The string " << needle << " found at offset "
                  << it - in.begin() << '\n';
    else
        std::cout << "The string " << needle << " not found\n";
}
```

### Ứng dụng

- Được sử dụng trong các trình chỉnh sữa văn bản (_text editor_) để tìm kiếm xâu.
- Là thuật toán được sử dụng trong _GNU's grep_

### Tham khảo

1. https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_string-search_algorithm
2. https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore%E2%80%93Horspool_algorithm
3. https://en.cppreference.com/w/cpp/utility/functional/boyer_moore_searcher
4. https://en.cppreference.com/w/cpp/utility/functional/boyer_moore_horspool_searcher