### Hướng dẫn giải một số bài tập
- http://acm.timus.ru/problem.aspx?space=1&num=1423&locale=en
  - Dễ thấy nếu chuỗi `T` là chuỗi `S` sau khi xoay vòng một số lần, thì việc ghép 2 chuỗi `T` lại với nhau
luôn luôn cho ra kết quả là một chuỗi chứa `S` (nếu không, in ra `-1`, bởi lẽ điều này luôn đúng). Ta chỉ
đơn giản tìm ra vị trí của `S` trong chuỗi đã ghép đó.
  -  Code mẫu : https://goo.gl/KDsUo5
    - (nếu link không hoạt động, truy cập
      https://gist.github.com/minhducsun2002/5b53cd628d27a2b85e702027e23ee082)

- http://codeforces.com/contest/625/problem/B
  - Bài này là bài dễ nhất. Tìm tất cả sự xuất hiện của chuỗi thứ hai trong chuỗi thứ nhất và vấn đề
được giải quyết.
  - Từ từ hãy làm. Cảnh báo trước, bài này yêu cầu các vị trí tìm thấy không được chồng lên nhau.
    Ví dụ với test `#5` :
    - `aaaaaaa` là văn bản, `aaaa` là mẫu khớp.
    - Mặc dù có thể tìm thấy 4 vị trí khớp nhau, tuy nhiên do các chuỗi chồng lên nhau nên không được tính :
         ```c
         aaaaaaa
         aaaa
          aaaa
           aaaa
            aaaa
          
    
    ..và vì thế đáp án cho test này chỉ là 1. Tương tự, test sau sẽ cho đáp án là 2:
    

         ```
         aaaaaaaa
         aaaa
             aaaa
         ```
