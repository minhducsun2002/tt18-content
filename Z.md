# Thuật Toán Z

## Giới thiệu

Thuật toán Z (Z algorithm) là thuật toán so khớp xâu, tương tự như KMP, với thời gian tuyến tính `O(m+n)`, với `n`, `m` là độ dài xâu văn bản và xâu mẫu.

## Đề bài

Ta sẽ lấy đề bài tương tự phần KMP, cho xâu văn bản `T` và xâu mẫu `P`, ta cần tính số lần xâu `P` xuất hiện trong `T`.

Trước hết, để hiểu thuật toán này, ta sẽ làm quen với mảng `Z`.

## Mảng Z

### Khái niệm

Mảng `Z` của một xâu `str` là mảng có cùng độ dài với xâu, với `Z[i]` là độ dài xâu con dài nhất của `str` bắt đầu tại `i`, và cũng là tiền tố của `str`.

Lưu ý là ta không xét `Z[0]` vì toàn bộ một xâu cũng chính là tiền tố của nó.

Ví dụ:

```
Vị trí: " 0  1  2  3  4  5  6  7  8  9  10 11"

 str:   " a  a  b  c  a  a  b  x  a  a  a  z"

  Z:    " X  1  0  0  3  1  0  0  2  2  1  0"

  => Z = {X,1,0,0,3,1,0,0,2,2,1,0}
```

### Cách lập

Ta có thể lập mảng `Z` với thời gian tuyến tính như sau:

Ta lưu một khoảng [l,r] với r là tối đa sao cho str(l..r) là tiền tố của xâu.

Khởi đầu tại l=0, r=0.

Từ đó ta có thể chạy biến i từ đầu đến cuối xâu như sau:

1. Nếu (i > r) => i nằm ngoài khoảng [l,r] nên ta đặt l=i, r=i
   xong tính r lớn nhất thỏa mãn str(l..r) là tiền tố của xâu,
   rồi lấy Z[i] là độ dài khoảng [l,r] hay Z[i] = r-l+1.

2. Nếu (i < r) => i nằm trong khoảng [l,r] (l luôn <= i), ta gọi
   k = i-l, hay k là vị trí của i trong xâu tiền tố str(l..r). Từ đó ta biết rằng
   str(k..r-l) = str(i..r) nên ta bỏ qua tính đoạn này bằng cách dùng Z[k]
   như sau:

   - Nếu Z[k] < r-i+1, chỉ có str(i..i+Z[k]) khớp với tiền tố,
     ta cho Z[i] = Z[k] luôn và không thay đổi l, r.

   - Nếu Z[k] >= r-i+1, ta biết s(i..r) khớp với tiền tố,
     nhưng không biết đằng sau r, nên ta đặt l = i, rồi
     từ đó ta tính r như trên, bỏ qua đoạn từ i đến r,
     xong lấy Z[i] = r-l+1 như bình thường

### Code tham khảo:

```cpp
string str;
vector<int> Z;
int len;

void getZ()
{
    len = str.size();
    Z.assign(len, int());
    int l = 0, r = 0;
    for (int i = 1; i < len; i++)               // lưu ý ta không tính Z[0]
    {
        if (i > r)                              //i > r
        {
            l = i, r = i;
            while (str[r] == str[r - l])
            {
                ++r;
            }
            Z[i] = r - l;
            --r;
        }
        else                                    //i <= r
        {
            int k = i - l;
            if (Z[k] < r - i + 1)               //Z[k] < r-i+1
            {
                Z[i] = Z[k];
            }
            else
            {
                l = i;
                while (str[r] == str[r - l])    //Z[k] >= r-i+1
                {
                    ++r;
                }
                Z[i] = r - l;
                --r;
            }
        }
    }
    return;
}
```

Vậy ta sử dụng mảng `Z` để giải bài toán trên như thế nào?

## Áp dụng

Gọi độ dài xâu `T` là `n`, độ dài xâu `P` là m.

Ta sẽ lập một xâu khác, giả sử là `s`, và đặt `s=P$T`, với `P` và `T` là xâu mẫu và `$` là kí tự không xuất hiện ở cả 2 xâu.

Ví dụ:

```
P = "aab", m = 3

T = "ababaabb", n = 8

=> S = "aab$ababaabb"
```

Giờ xét mảng `Z` của xâu `S` ở trên:

```
Vị trí: " 0  1  2  3  4  5  6  7  8  9  10 11"

  S:    " a  a  b  $  a  b  a  b  a  a  b  b "

  Z:    " X  1  0  0  1  0  1  0  3  1  0  0 "
```

Nhận xét: Giá trị các phần tử của `Z` sẽ không vượt quá `m`, vì ta chắc chắn không có giá trị nào giống `$`, nên sẽ không có xâu con nào có độ dài lớn hơn `m` và cũng là tiền tố của `S`.

Từ đây ta dễ dàng thấy được, các giá trị `Z[i]` có giá trị bằng `m` chứng tỏ xuất hiện xâu `P` tại vị trí `i`, và từ đó có thể tính được các vị trí xuất hiện của xâu `P` trong xâu `T`.

Đây là code mẫu đầy đủ của bài trên để các bạn tham khảo:

```cpp
#include <vector>
#include <iostream>
#include <stdio.h>

using namespace std;

typedef vector<int> vi;

string S, P, T;
vi Z;

void getZ() //Lập mảng Z cho S
{
    Z.assign(S.size(), int());
    int l = 0, r = 0;
    for (int i = 1; i < S.size(); i++)
    {
        if (i > r)
        {
            l = i, r = i;
            while (S[r] == S[r - l])
            {
                ++r;
            }
            Z[i] = r - l;
            --r;
        }
        else
        {
            int k = i - l;
            if (Z[k] < r - i + 1)
            {
                Z[i] = Z[k];
            }
            else
            {
                l = i;
                while (S[r] == S[r - l])
                {
                    ++r;
                }
                Z[i] = r - l;
                --r;
            }
        }
        if (Z[i] == P.size()) //Tại đây ta kiểm tra Z[i] và output luôn
        {
            cout << i - P.size() << " "; //Lưu ý vị trí
        }
    }
    return;
}

void findpattern(string P, string T)
{
    S = P + '$' + T; //Lập S
    getZ();
}

int main() //Nhập p và t
{
    cin >> P >> T;
    findpattern(P, T);
}
```

## Tham khảo

1. https://www.hackerearth.com/practice/algorithms/string-algorithm/z-algorithm/tutorial/

<img src="./img/Z-ref-1.svg" width=120px>

2. http://iq.opengenus.org/z-algorithm-function/

<img src="./img/Z-ref-2.svg" width=120px>