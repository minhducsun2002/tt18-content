# String Matching

<!-- naive.md -->
<!-- Written by Nguyen Tuan Dung <@dungwinux> -->

## The Naïve Way

Để bắt đầu cho String-Matching, chúng ta sẽ khởi động với phương thức cơ bản
nhất, gọi là Naïve String Matching.

Nói ngắn gọn, thuật này rất đơn giản, bạn đọc có thể gập sách lại cài thử. Nếu
bạn quá bí, thì sau đây là code mẫu:

```cpp
#include <iostream>
#include <string>

int naive_search(std::string target, std::string pattern)
{
    for (int i = 0; i <= target.size() - pattern.size(); ++i)
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
