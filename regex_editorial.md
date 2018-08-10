### Hướng dẫn giải cho một số bài tập :

> **Chú ý :** hàm `std::regex_match` sẽ khớp **cả chuỗi**,
> còn `std::regex_search` sẽ có thể khớp **cả chuỗi** và **bất kỳ chuỗi con nào**.

#### Codeforces 58A

Dễ thấy, để từ được nhập vào (tạm ký hiệu là `N`) được coi là `hello`, thì từ đó phải chứa `hello` như là một dãy con.
Có 2 hướng giải cho bài này :

- Dùng Longest Common Subsequence (dãy con chung dài nhất) để so sánh `N` với `hello`.

  Nếu LCS của cả 2 chuỗi là `hello` thì chứng tỏ chuỗi ban đầu chứa `hello`.

- Dùng regular expression (mục đích chính khi ra bài này).

  Dễ thấy, nếu `N` chứa `hello` như là một chuỗi con,
  thì giữa mỗi ký tự của `hello` sẽ có thể có ký tự khác chèn vào.

  Biểu thức để khớp với mọi chuỗi (sử dụng ký tự Latin) là `.*`,
  do đó để khớp một chuỗi con có `he` là một dãy con chẳng hạn, ta dùng biểu thức `h.*e`.

  Đó là ý tưởng chính để giải bài này.

#### Codeforces 96A

Nếu chuỗi nhập vào chứa ít nhất 7 ký tự `0` hoặc `1` đứng cạnh nhau thì in ra `YES`.
Đây chính là điểm mấu chốt, vì chúng ta chỉ cần ghép 7 ký tự liền nhau.
Dễ thấy, có 2 cách giải :

- Đơn giản nhất, khớp chuỗi nhập vào với 7 chữ số `0` hoặc `1` liền nhau
  (**bằng hàm so khớp, không phải regex**).
  Do bài này giới hạn khá nhỏ (độ dài không quá `100`),
  nên việc so khớp một cách trâu bò là khả thi với
  độ phức tạp tính toán cao nhất **chỉ** là `O((100 - 93 + 1) * 7)` hay `O(94 * 7)`

  Code mẫu :

  https://codeforces.com/contest/96/submission/38828432
  (sử dụng `std::string::find`, hàm trâu bò có sẵn)

  <img src="./img/regex-e-0.svg" width=120px>

- Dùng regular expression.

  Dễ thấy, để khớp 7 ký tự liền nhau ta sử dụng `x{7}` với `x` là ký tự cần khớp.

  Các ký tự cần khớp ở đây là `0` \_\_hoặc `1`, do đó ta có 2 biểu thức là `0{7}` và `1{7}`.

  Tuy nhiên, đề bài chỉ yêu cầu khớp 1 trong 2 trường hợp đó, thế nên chúng ta sẽ chèn
  dấu `|` vào giữa (biểu thị cho toán tử `OR`).

  Code mẫu :

  https://codeforces.com/contest/96/submission/41415196 (sử dụng `std::regex_match`)

  <img src="./img/regex-e-1.svg" width=120px>

  https://codeforces.com/contest/96/submission/41415353 (sử dụng `std::regex_search`)

  <img src="./img/regex-e-2.svg" width=120px>
