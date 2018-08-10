# String Matching

## Thuật toán Knuth-Morris-Pratt

### Một trong những thuật toán tìm kiếm chuỗi tối ưu nhất 

Bài toán tìm kiếm (so khớp) chuỗi, hay còn gọi là bài toán needle-in-haystack
(cây kim trong đống rơm), là bài toán mà trong đó chúng ta được cho một văn bản
(tạm ký hiệu dưới dạng một chuỗi T), và một chuỗi mẫu (pattern, tạm ký hiệu là
chuỗi P). Nhiêm vụ đơn giản là tìm ra mọi sự xuất hiện của P trong T.

Thuật toán Knuth-Morris-Pratt là một thuật toán chạy với thời gian tuyến tính
để giải quyết bài toán này, được xây dựng dựa trên nhận xét rằng một mẫu khớp
bất kỳ sẽ chứa đủ thông tin để chỉ ra vị trí cần được so sánh tiếp theo
sau mỗi lần khớp, từ đó không so sánh lại bất kỳ ký tự nào đã được so sánh trước
đó.

Thuật toán được tìm ra lần đầu vào năm 1970 bởi Donald E. Knuth và học trò
của mình là Vaughan R. Pratt. James H. Morris cũng tìm ra thuật toán này một 
cách độc lập cùng năm. Đây là thuật toán tìm kiếm chuỗi đầu tiên có thời gian
thực thi tuyến tính.

---
Ghi chú:
- ký hiệu `|` giữa 2 ký tự chỉ ra rằng 2 ký tự đó được biết khớp nhau thông qua
so sánh
- ký hiệu `\` giữa 2 ký tự chỉ ra rằng 2 ký tự đó được biết khớp nhau __không__
thông qua so sánh
- ký hiệu `-` giữa 2 ký tự chỉ ra rằng 2 ký tự đó đang được so sánh
- ký hiệu `×` giữa 2 ký tự chỉ ra rằng 2 ký tự đó không khớp nhau
---

### Đề

Trước tiên, chúng ta có thuật toán khá là trâu bò như sau:

```cpp
match(T, P)
{
    T_s = T.size(); P_s = P.size();
    for i = 0 to T_s - P_s + 1
        // với mỗi bước dịch
        match = true;
        for n = 0 to P_s - 1
            if (P[n] != T [n + i])  
            // nếu 2 ký tự không khớp
                match = false; break;
                // thì cả đoạn đang xét không khớp
        if (match == true) return i;
        // nếu khớp trả về vị trí của mẫu tìm thấy trong văn bản
    return NO_MATCH;
}
```

Thuật toán này có độ phức tạp thời gian là `O(mn)` với m, n lần lượt là độ dài
của văn bản và chuỗi mẫu.

Dễ thấy thuật toán này đang so sánh lại những ký tự đã được so sánh (và khớp)
trước đó. Cụ thể hơn, sau mỗi vòng lặp, chúng ta chỉ dịch đi một ký tự và bắt
đầu lại. Giả sử với văn bản là `AAAAAAA` và mẫu là `AAA`, chúng ta sẽ cần tới
`7 * 4 = 28` phép so sánh (và phần nhiều trong số đó là không cần thiết). 

Ý tưởng của thuật toán Knuth-Morris-Pratt (từ đây viết tắt là KMP) là dịch
phần văn bản để so sánh đi xa hơn.

Xét ví dụ dưới đây:

- Văn bản: `AAAAAAAA`, tạm ký hiệu là `T`
- Chuỗi cần tìm: `AAA`, tạm ký hiệu là `P`
Với phương pháp so khớp thông thường, chúng ta sẽ cần sử dụng đến
`8 * 4` phép so sánh.

Tuy nhiên, thuật toán KMP sẽ sử dụng ít phép so sánh hơn (và do đó tối ưu hơn).

Để ý thấy trong chuỗi cần tìm, ta có `P[0] = P[1]` và `P[1] = P[2]` (nói cách
khác, `P[0..1] = P[1..2]`)


Sau lần kiểm tra đầu tiên, ta có 
```
AAAAAAAA
|||
AAA
```
hay `T[0..1] = P[0..1]`, `T[1..2] = P[1..2]`.

Tiếp tục khớp ở vị trí số 2, ta thấy `T[1..2]` đã được khớp trước đó với
`P[1..2]`.

Mà `P[0..1] = P[1..2]`, do đó `T[1..2] = P[0..1]` → ta chỉ cần so sánh
`P[2]` với `T[3]` để kết luận liệu `T[1..3]` có khớp với `P[0..2]` hay không.

Tiếp tục làm tương tự với các trường hợp còn lại, chúng ta thu được đáp án
(`P` xuất hiện trong `T` ở các vị trí `0`, `1`, `2`, `3`, `4`, `5`). KMP
chỉ cần sử dụng 3 phép so sánh cho lần khớp đàu tiên và 1 phép so sánh cho
mỗi lần lặp sau, tối ưu hơn rất nhiều so với phương pháp trâu bò kể trên.

Và đây cũng là cách con người xử lý bài toán.

Một ví dụ khác:
- `T = AACABCAAABAAA`
- `P = AABA`

Ta thấy `P[2] != T[2]`. Dịch đi một ký tự, ta lại có
```
AACABCAAABAAA
 |×
 AABA
```
hay `P[1] != T[2]`.

Thông thường, thuật toán trâu bò sẽ tiếp tục dịch đi một phần tử, so sánh lại
(và lại mismatch D:). Tuy nhiên, nếu để ý ta thấy: `P[0]` giống `P[1]`, do đó
việc so sánh `P[0]` với `T[2]` tiếp là không tối ưu cho lắm. Ta lại tiếp tục
dịch đi (mà không so sánh) và tiếp tục khớp `T[3]` với `P[0]`.

Ví dụ cuối cùng:
- `T = EXTENDEXPANDEXECUTE`
- `P = EXTENDEXT`

Để ý rằng `P[0..1] = P[6..7]`.

Chúng ta tiếp tục so sánh:
```
EXTENDEXPANDEXECUTE
||||||||×
EXTENDEXT
```
Thuật toán trâu bò (một lần nữa) sẽ lại dịch đi một ký tự và so sánh `P[0]` với `T[1]`. 

Tuy nhiên, dễ thấy `T[6..7] = P[6..7]`.

Trước đó ta lại có `P[0..1] = P[6..7]`, do đó thuật toán sẽ tiếp tục tìm kiếm ở `T[8]`
(vì `T[6..7]` đã khớp trước đó):
```
EXTENDEXPANDEXECUTE
      \\-
      EXTENDEXT
```
Bằng cách đó, chúng ta sẽ giảm đi được kha khá số phép so sánh cần dùng (nhất là khi
mẫu khớp của chúng ta dài).

### Thực
Một câu hỏi đặt ra cho chúng ta là: làm thế nào để biết được số vị trí cần di chuyển?

> Đến đây người đọc nên nhắm mắt lại, suy nghĩ, và (nếu không nghĩ được, nghĩ xong rồi, ...) đọc tiếp xuống dưới.

Hãy để ý rằng với mỗi tiền tố của `P` (tạm ký hiệu là `P(i)`), tồn tại một giá trị `n` mà tiền tố độ dài `n`
của `P(i)` tương đương với hậu tố độ dài `n` của chính nó.

Đó cũng chính là giá trị cho ta biết được tiếp theo nên tìm kiếm ở đâu.

Để hiểu rõ hơn, chúng ta sẽ xem ví dụ sau :

```
MAGICMAGEMAGISK
||||||||×
MAGICMAGISK
```

Tiền tố đã được khớp là `MAGICMAG`.

Dễ thấy, tiền tố độ dài `3` của `MAGICMAG` khớp với hậu tố độ dài `3` của chính nó.
Vì thế, chúng ta sẽ tiếp tục tìm như sau :
```
MAGICMAGEMAGISK
     \\\-
     MAGICMAGISK
```

Xét một ví dụ khác :
```
PREFPITCHPERFECTION
|||||×
PREFPCTPREFECT
```

Tiền tố đã được khớp là `PREFP`.

Tiền tố độ dài `1` khớp với hậu tố độ dài `1`, vì thế ta sẽ tiếp tục tìm ở `T[5]`:
```
PREFPITCHPERFECTION
    \-
    PREFPCTPREFECT
```

Từ đó ta thấy :
Với mỗi tiền tố của `P` (ký hiệu `P(i)` với `i` là độ dài tiền tố), ta có `n` là độ dài tiền tố chuẩn nhỏ nhất
mà bản thân tiền tố đó cũng là hậu tố chuẩn của `P(i)`. `n` càng lớn, số phép so sánh
có thể bỏ qua càng cao.

(Tiền/hậu tố chuẩn là tiền/hậu tố có độ dài nhỏ hơn bản thân chuỗi đó. Chúng ta sử dụng
tiền/hậu tố chuẩn bởi lẽ, với tiền tố là cả xâu thì việc xử lý không còn ý nghĩa nữa.)

Xét ví dụ thứ ba ở trên (`T = EXTENDEXTENTEXECUTE`, `P = EXTENDEXT`).

Gọi `P(i)` là tiền tố độ dài `i` của `P`.

Gọi `N(i)` là độ dài tiền tố chuẩn dài nhất mà cũng là hậu tố chuẩn của `P(i)`.

Ta có bảng `N(i)` như sau :

| `i`   | `P(i)`      | `N(i)` |
| :---: | :---------: | :----: |
| `1`   | `E`         | `0`    |
| `2`   | `EX`        | `0`    |
| `3`   | `EXT`       | `0`    |
| `4`   | `EXTE`      | `1`    |
| `5`   | `EXTEN`     | `0`    |
| `6`   | `EXTEND`    | `0`    |
| `7`   | `EXTENDE`   | `1`    |
| `8`   | `EXTENDEX`  | `2`    |
| `9`   | `EXTENDEXT` | `3`    |

Hãy xem cách chúng ta dùng bảng sau đây vào quá trinh so sánh :
```
EXTENDEXTENTEXECUTE
||||||||
EXTENDEXT
```

Ta có `N(i) = 3`, do đó 3 ký tự tương ứng (ở cuối `P`) trong `T` sẽ không cần phải so sánh.
```
EXTENDEXTENTEXECUTE
      \\\-
      EXTENDEXT
```

Tương tự :
```
EXTENDEXTENTEXECUTE
      \\\||×
      EXTENDEXT
```

Ta có `N(6) = 0`, do đó chúng ta không dịch được thêm ký tự nào :
```
EXTENDEXTENTEXECUTE
       -
       EXTENDEXT
```

Tương tự như vậy đến hết.

> Ý tưởng chính của thuật toán :
> - Gọi `P(i)` là tiền tố độ dài `i` của mẫu khớp.
> - Gọi `N(i)` là độ dài tiền tố chuẩn nhỏ nhất của `P(i)`  mà bản thân tiền tố
> đó cũng là hậu tố (chuẩn) của `P(i)`.
> - Bảng các giá trị `N(i)` sẽ cho ta biết sau một lần khớp, vị trí tối ưu để
> tiếp tục khớp là ở đâu.

### Luận

Giờ chúng ta sẽ nói đến điều quan trọng nhất, thứ sẽ quyết định khả năng áp dụng vào thực tế
của một thuật toán: độ phức tạp bộ nhớ và độ phức tạp tính toán.

Trước tiên, nói về độ phức tạp bộ nhớ, ta thấy thuật toán yêu cầu một mảng có độ lớn tương đương
mẫu khớp để chứa các giá trị tiền xử lý và không yêu cầu gì thêm, do đó, độ phức tạp bộ nhớ là
`O(m)` với `m` là độ dài mẫu khớp.

Phần chính ở đây là về độ phức tạp tính toán :

Việc so khớp sau khi đã có bảng phương án, dễ thấy, có độ phức tạp `O(n)`, do mỗi ký tự chỉ
được khớp một lần (toàn bộ các phép so sánh thừa đều được bỏ đi do có tiền xử lý).

Tuy vậy, việc tính bảng phương án cũng tốn thời gian (dĩ nhiên).

Một phương pháp tính bảng `N(i)` ở trên là tăng dần độ dài tiền/hậu tố và so sánh tiền/hậu tố đó
liên tục đến khi có kết quả. Tuy nhiên, dễ thấy phương pháp này không khả dụng khi độ dài mẫu khớp
lớn (trên `10000` chẳng hạn) bởi độ phức tạp tiệm cận `O(n * n)` của nó.

> Đến đây, người đọc được khuyến khích nhắm mắt lại và suy nghĩ một phương thức tính toán tối ưu nhất.

> Bảng phương án có thể được tính trong thời gian `O(m)`, với `m` là độ dài mẫu khớp.

Dưới đây sẽ trình bày phương pháp đó.

Xét mẫu khớp `ABCDABD`.

Gọi `length` là giá trị `N(i)` tương ứng hiện tại, với `i` là độ dài tiền tố đang xét của `ABCDABD`.

| `length` | `0`   |       |       |       |       |       |       |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| `i`      | `0`   |       |       |       |       |       |       |
|          | `A`   | `B`   | `C`   | `D`   | `A`   | `B`   | `D`   |
| `N(i)`   |       |       |       |       |       |       |       |

Với `i = 0`, dĩ nhiên không tồn tại tiền tố chuẩn nào có độ dài nhỏ hơn `1` mà lớn hơn `0`, do đó `N(0) = 0`.

Ta tăng `i` và nhận thấy, `P[1]` khác `P[0]`, do đó `N(1) = 0`.

Tại `i = 3`, ta có :

| `length` | `0`   | `0`   | `0`   | `0`   |       |       |       |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| `i`      | `0`   | `1`   | `2`   | `3`   |       |       |       |
|          | `A`   | `B`   | `C`   | `D`   | `A`   | `B`   | `D`   |
| `N(i)`   | `0`   | `0`   | `0`   | `0`   |

Tại `i = 4`, ta thấy `P[i] == P[0]`, do đó ta tăng giá trị của `length` lên. Tương tự với `i = 5`.

| `length` | `0`   | `0`   | `0`   | `0`   | `1`   | `2`   |       |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| `i`      | `0`   | `1`   | `2`   | `3`   | `4`   | `5`   |       |
|          | `A`   | `B`   | `C`   | `D`   | `A`   | `B`   | `D`   |
| `N(i)`   | `0`   | `0`   | `0`   | `0`   | `1`   | `2`   |

Tại `i = 6`, do `P[6]` khác `P[0]`, nên ta sẽ lùi giá trị của `length` về `N(i)` gần trước đó thu được
(tức `length = N(i)[length - 1]`) :

| `length` | `0`   | `0`   | `0`   | `0`   | `1`   | `2`   | `0`   |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| `i`      | `0`   | `1`   | `2`   | `3`   | `4`   | `5`   |       |
|          | `A`   | `B`   | `C`   | `D`   | `A`   | `B`   | `D`   |
| `N(i)`   | `0`   | `0`   | `0`   | `0`   | `1`   | `2`   | `0`   |
|          |       | ↑     |       |       |       |       | ‖     |
|          |       | =     | ==    | ==    | ==    | ==    | =     |

Dễ thấy, chúng ta chỉ tính toán tại mỗi giá trị của `i` một lần, do đó độ phức tạp của phương pháp này là `O(m)`.

→ Độ phức tạp tính toán của KMP là `O(m + n)`.

### Kết
Thuật toán Knuth-Morris-Pratt rất được ưa chuộng trong nhiều lời giải cho các bài tập lập trình,
bởi tốc độ của thuật toán này hiện nay chưa có một thuật toán nào có thể vượt qua (về mặt trung bình).
Tuy nhiên, điều làm cho KMP khó phổ biến là cách cài đặt của nó rất khó thuần thục : độ phức tạp
có thể tăng từ `O(m + n)` lên `O(m * m + n)` nếu phương pháp cài đặt không hiệu quả.

Ưu điểm :
- Tốc độ thực thi không-có-đối-thủ với độ phức tạp tính toán luôn luôn ở mức `O(m + n)`
  - Để so sánh, thuật toán Boyer-Moore có thời gian tiền xử lý `O(m + k)` (với `k` là độ lớn bảng chữ cái)
và tốc độ thực thi tốt nhất là `Ω(n / m)`, nhưng có thể lên đến `O(m * n)` ở trường hợp tệ nhất, khi mẫu khớp
và văn bản đều là sự lặp lại của cùng một ký tự).
  - Thuật toán Rabin-Karp có tốc độ trung bình cũng là `O(m + n)`, tuy nhiên tồn tại trường hợp tệ nhất với độ
phức tạp lên đến `O((n - m) * m)`
- Yêu cầu bộ nhớ `O(m)` giống như hầu hết các thuật toán khác, do đó hầu hết bài tập sẽ có thể được giải
với thuật toán này.

Nhược điểm :
- Khó cài đặt (và dĩ nhiên là khó gỡ lỗi) bởi phương pháp so sánh tương đối phức tạp, đặc biệt khi
so sánh với những thuật toán khác như Z và Rabin-Karp. Bên cạnh đó, hàm tiền xử lý nếu không được tối ưu
sẽ tiềm tàng nguy cơ chạy quá thời gian quy định.
- Không tối ưu đối với các trường hợp cần khớp nhiều mẫu một lúc. Giải pháp lúc này là thuật toán Aho-Corasick,
vốn là một sự mở rộng của bản thân KMP.