# Thuật Toán Z

## Giới thiệu

Thuật toán Z (Z algorithm) là thuật toán so khớp xâu, tương tự như KMP, với thời gian tuyến tính `O(m+n)`, với `n`, `m` là độ dài xâu văn bản và xâu mẫu.

## Đề bài

Ta sẽ lấy đề bài tương tự phần KMP, cho xâu văn bản `T` và xâu mẫu `P`, ta cần tính số lần xâu `P` xuất hiện trong `T`.

Trước hết, để hiểu thuật toán này, ta sẽ làm quen với mảng `Z`.

## Mảng Z
### Khái niệm

Mảng `Z` của một xâu `Str` là mảng có cùng độ dài với xâu, với `Z[i]` là độ dài xâu con dài nhất của `Str` bắt đầu tại `i`, và cũng là tiền tố của `Str`.

Lưu ý là ta không xét `Z[0]` vì toàn bộ một xâu cũng chính là tiền tố của nó.

Ví dụ:
```
Vị trí: " 0  1  2  3  4  5  6  7  8  9  10 11"

 Str:   " a  a  b  c  a  a  b  x  a  a  a  z"
  
  Z:    " X  1  0  0  3  1  0  0  2  2  1  0"
  
  => Z = {X,1,0,0,3,1,0,0,2,2,1,0}
```

### Cách lập

Vậy lập mảng `Z` như thế nào?

Ta có thể lập mảng `Z` với thời gian tuyến tính như sau:

```
Ta lưu một khoảng [l,r] với r là tối đa sao cho str(l..r) là tiền tố của xâu.

Khởi đầu tại l=0, r=0.

Từ đó ta có thể chạy biến i từ đầu đến cuối xâu như sau:

1. Nếu (i > r) => i nằm ngoài khoảng [l,r] nên ta đặt l=i, r=i
   xong tính r lớn nhất thỏa mãn str(l..r) là tiền tố của xâu,
   rồi lấy Z[i] là độ dài khoảng [l,r] hay Z[i] = r-l+1.
   
2. Nếu (i < r) => i nằm trong khoảng [l,r] (l luôn <= i), ta gọi
   k = i-l, hay k là vị trí của i trong xâu tiền tố [l,r]. Từ đó ta biết rằng
   str(k..r-l) = str(i..r) nên ta bỏ qua tính đoạn này bằng cách dùng Z[k]
   như sau:
   
   1. Nếu Z[k] < r-i+1, chỉ có str(i..i+Z[k]) khớp với tiền tố,
      ta cho Z[i] = Z[k] luôn và không thay đổi l, r.
      
   2. Nếu Z[k] >= r-i+1, ta biết s(i..r) khớp với tiền tố,
      nhưng không biết đằng sau r, nên ta đặt l=i, rồi từ
      đó ta tính r lớn nhất thỏa mãn, xong lấy Z[i] = r-l+1
      như bình thường
```

Code mẫu:
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

Ta sẽ lập một xâu khác, giả sử là `s`, và đặt `s=P$T`, với `P` và `T`  là xâu mẫu và `$` là kí tự không xuất hiện ở cả 2 xâu.

Ví dụ:
```
P="aab", m=3

T="ababaabb", n=8

=> S="aab$ababaabb"
```

Giờ xét mảng `Z` của xâu `S` ở trên:
```
Vị trí: " 0  1  2  3  4  5  6  7  8  9  10 11"

  S:    " a  a  b  $  a  b  a  b  a  a  b  b "
 
  Z:    " X  1  0  0  1  0  1  0  3  1  0  0 "
```
Nhận xét: vì có kí tự `$`, nên giá trị các phần tử của `Z` sẽ không vượt quá `m`, vì ta chắc chắn không có giá trị nào giống `$`, nên sẽ không có xâu con nào có độ dài lớn hơn `m` và cũng là tiền tố của `S` ngoài chính tiền tố của nó, hay `Z[0]` mà ta không xét đến. Vì vậy nên tiền tố lớn nhất có thể chính là xâu `P`.

Từ đây ta dễ dàng thấy được, các giá trị `Z[i]` có giá trị bằng `m` chứng tỏ xuất hiện xâu `P` tại vị trí `i`, và từ đó có thể tính được các vị trí xuất hiện của xâu `P` trong xâu `T`.


