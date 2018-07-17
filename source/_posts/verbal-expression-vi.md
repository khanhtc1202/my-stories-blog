---
title: Từ bỏ Regex VerbalExpressions
date: 2018-07-03 23:47:27
tags: technical
thumbnail: https://i.imgur.com/nMveuD7.png
---


Chắc hẳn bất kỳ 1 lập trình viên nào cũng đã ít nhất 1 lần gặp phải regular expressions - regex (hay còn gọi biểu thức chính quy). 
Regex dùng để tìm kiếm, sửa đổi, thao tác trên văn bản; được nhiều ngôn ngữ hỗ trợ
 và là 1 công cụ không thể thiếu được trong việc xử lý chuỗi. Thế nhưng
cú pháp của Regex lại rối mắt; khó đọc và khó hiểu; cản trở đến việc sử dụng regex khá nhiều. Ví dụ khi cần kiểm tra 1 chuỗi có phải 
là 1 địa chỉ url hợp lệ hay không thì có thể sử dụng đoạn regex này `/^(https?:\/\/)[^ ]+$/` 

![](https://i.imgur.com/nMveuD7.png)

Chưa kể đến việc số lần cần sử dụng regex trong công việc là tương đối ít; chi phí RoI (return-on-investment) là quá lớn. 
Đến 80% những chuỗi regex thường hay sử dụng đều có thể tìm thấy dễ dàng trên mạng (mật khẩu, url, ip,...). 
Liệu bạn có sẵn sàng bỏ ra 1 vài tuần để học 1 thứ mà bạn biết là sẽ cực kỳ ít sử dụng 
và nhiều khả năng có thể tìm thấy câu trả lời ở trên internet không?

Tuy nhiên nếu có 1 dự án nào đó yêu cầu 1 trong những chuỗi regex nào đó không có trên mạng thì sao?
Nhiều khả năng bạn gặp bí; và chắc hẳn lúc đó bạn sẽ hối hận vì đã không học regex tử tế hơn.

Nếu trót lười rồi, mà vẫn không muốn hối hận. Thì có sẵn giải pháp thần kỳ hơn là VerbalExpressions. Hãy cùng
thử nhìn vào đoạn code này nhé.

```
VerEx()
.startOfLine()
.then('http')
.maybe('s')
.then('://')
.anythingBut(' ')
.endOfLine();
```

Nhìn qua là các bạn có thể đoán được công dụng của đoạn code này rồi; 
đầu tiên là 1 dòng mới, yêu cầu có `http`, có thể có `s` hoặc không (`http` hay `https` đều được),
cần thêm `://`, sau đó bất kỳ ký tự nào miên không phải khoảng trống. Vậy là ta đã có thể match được 1 đoạn url rồi.
Regex tương ứng với đoạn code trên là `/^(?:http)(?:s)?(?:\:\/\/)(?:[^ ]*)$/`, regex được sinh ra tuy có 
khác với đoạn regex trên nhưng công dụng hoàn toàn như nhau.

VerbalExpressions đã giải quyết được vấn đề lớn nhất của regex là khó đọc, khó nhớ. Như là thay vì phải viết 
query SQL thì bạn dùng ORM vậy. Cú pháp đơn giản, dễ dùng và quen thuộc với lập trình viên. 
Ngoài ra thư viện này đã được port sang hầu hết các ngôn ngữ phổ biển (Java, C, PHP,...); hoàn toàn có thể yên tâm
mà bỏ qua việc học regex. 

Các bạn có thể viết thử và compile thành regex tại [verbalregex.com](https://verbalregex.com/).
Trong trường hợp không muốn cài thêm thư viện vào project của mình thì cũng có thể vào site trên; 
viết 1 đoạn VerbalExpressions, biên dịch và copy đoạn regex sinh ra và sử dụng xong project của mình. Rất nhanh và tiện lợi!
  
  
Have fun with VerbalExpressions :)

---

Ver 1.1

Một trong những đối tượng chính của programming là `string`, khi tiếp cận với bất cứ ngôn ngữ nào mới, cái đầu tiên mà bạn sẽ tìm kiếm, có lẽ sẽ theo thứ tự dạng như: variable, assignment, operation (number/string), ... Rõ ràng ai cũng có thể thấy tầm quan trọng của string đối với công cuộc kiếm cơm của dev chúng ta :))

Những tác vụ xử lý trên string có thể đơn giản liệt kê ra như: check match, replace, ...hầu như đa phần trong số các tác vụ đó, vấn đề nổi lên cần giải quyết là, bạn phải tìm cách xác định được pattern của string đầu vào, xác định xem thành phần nào của string mà bạn sẽ tương tác. Tất nhiên khi mới bắt tay học lập trình, có lẽ một điều chắc chắn, bạn sẽ cố viết 1 vài đoạn code kiểu như:

```C
int checkMatchStubPattern(char* string) {
    for(int i = 0; i < strlen(string); i++) {
        // logic for checking string pattern
        ...
    }
    return ...
}
```

Tất nhiên, không ai có thể thể nói cách làm của bạn không đúng - miễn là bạn có thể giải quyết vấn đề của bạn :)) Tuy nhiên trong thực tế những pattern trên string khá đa dạng và cần nhiều xử lý đặc biệt. Bạn sẽ phải code nhiều hơn, điều này tương đương với nguy cơ bug nhiều hơn, khó maintain hơn,... Và người ta đã tạo ra `Regular Expression - Regex` để giải quyết vấn đề đó. Đây quả thật là một phát minh lớn, thay đổi hoàn toàn cách mà chúng ta tương tác với string. Dẫn chứng đơn giản nhất thể hiện sự tuyệt vời của nó là hầu như mọi ngôn ngữ hiện tại đều support sử dụng regex trong xử lý string.

Regex quả là 1 cách hiệu quả để giải quyết vấn đề liên quan đến string , thế nhưng regex lại đi kèm đến một vấn đề khác là nó thực sự khó tiếp cận (có lẽ do bản thân các pattern trên string khá phức tạp, bạn có thể nhận ra phần nào dựa vào ví dụ dưới đây).

![Đoạn regex để kiểm tra 1 chuỗi có phải là url hay không](https://i.imgur.com/nMveuD7.png)

Cú pháp rối mắt; khó đọc và khó hiểu là lý do lớn cản trở đến việc sử dụng regex. Chưa kể đến việc số lần cần sử dụng regex trong công việc là tương đối ít; chi phí RoI (return-on-investment) là quá lớn. Đến 80% những chuỗi regex thường hay sử dụng đều có thể tìm thấy dễ dàng trên mạng (mật khẩu, url, ip,...). Liệu bạn có sẵn sàng bỏ ra 1 vài tuần để học 1 thứ mà bạn biết là sẽ cực kỳ ít sử dụng và nhiều khả năng có thể tìm thấy câu trả lời ở trên internet không? Duy trì tư duy đó dẫn đến đa số việc các lập trình viên có xu hướng đi copy các đoạn regex sẵn có để sử dụng trong dự án của mình; ngay cả khi chuỗi đó không tồn tại trên mạng thì cũng cố gắng tìm 1 chuỗi gần đúng với yêu cầu nhất và sửa lại cho phù hợp với nhu cầu của mình. Thậm chí có trường hợp mất đến vài tiếng đến cả ngày trời tìm ra được 1 đoạn regex gần giống và sửa lại thay vì mất 1 tiếng tập trung nghiên cứu và tự viết đoạn regex phù hợp với yêu cầu của mình.

![In every angel a demon hides...](https://i.imgur.com/j3G9xyP.png)


Để giải quyết vấn đề đó thì có 1 cách khác hiệu quả hơn là [`Verbal Expressions`](http://verbalexpressions.github.io/); đây là 1 thư viện giúp việc xây dựng những đoạn regex phức tạp. Thư viện mã nguồn mở hỗ trợ hầu hết các ngôn ngữ phổ biến (Java, C, JS...); được cộng đồng đánh giá rất cao (JSVerbalExpressions có trên 10k stars). 

``` javascript
VerEx()
.startOfLine()
.then('http')
.maybe('s')
.then('://')
.anythingBut(' ')
.endOfLine();
``` 

Nhìn qua cũng có thể đoán được công dụng của đoạn code trên; đầu tiên là 1 dòng mới, yêu cầu có `http`, có thể có `s` hoặc không (`http` hay `https` đều được), cần thêm `://`, sau đó bất kỳ ký tự nào miễn không phải khoảng trống. Vậy là ta đã có thể match được 1 đoạn url rồi. Regex tương ứng với đoạn code trên là `/^(?:http)(?:s)?(?:\:\/\/)(?:[^ ]*)$/`, regex được sinh ra tuy có khác với đoạn regex trên nhưng công dụng hoàn toàn như nhau.

VerbalExpressions đã giải quyết được vấn đề lớn nhất của regex là khó đọc, khó nhớ. Độ tiện lợi cũng như chuyển từ viết query SQL sang dùng ORM vậy. Bên cạnh đó cú pháp đơn giản, dễ dùng và rất quen thuộc với lập trình viên; bạn có thể viết được 1 đoạn code sinh ra regex cần thiết chỉ sau ít phút làm quen với Verbal Expressions.

Tuy nhiên VerbalExpressions có 1 nhược điểm là cần phải import thư viện vào mới sử dụng được; việc này có thể khá mất công nếu chỉ dùng thư viện 1 lần. Chưa kể đối với những dự án mà khách hàng kiểm tra dependency sát sao thì có thể sẽ không được dùng. Trong trường hợp đó các bạn có thể sử dụng [verbalregex](https://verbalregex.com/) để code và sinh ra regex, sau đó sử dụng thư viện regex mặc định của ngôn ngữ là vấn đề sẽ được giải quyết.

![Công cụ online hỗ trợ xây dựng xây dựng regex](https://i.imgur.com/w0uMzYE.png)

Các bạn có thể sử dụng tool trên tại [verbalregex.com](https://verbalregex.com/).

VerbalExpressions không thể thay thế cho regex, cũng giống như ORM không thể thay cho những câu query được; chúng ta vẫn cần phải sử dụng regex (hay viết query) cho những trường hợp phức tạp. Tuy nhiên VerbalExpressions vẫn là 1 cách sử dụng regex gián tiếp hiệu quả và dễ dùng, có thể xây dựng phần lớn những đoạn regex "lạ" mà chúng ta bất ngờ gặp phải.
