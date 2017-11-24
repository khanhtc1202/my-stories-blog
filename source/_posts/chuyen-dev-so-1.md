---
title: Chuyện dev số 1 - pair deploying
date: 2017-11-24 22:27:48
tags: devstories
thumbnail: https://spin.atomicobject.com/wp-content/uploads/Pairing.jpg
---

1 ngày 24 tiếng thì mình ở công ty tầm 10 tiếng; tính ra chiếm khá nhiều thời gian của 1 ngày. Chưa kể tối về thỉnh thoảng đọc sách, code thêm,... Có 1 đợt tâm trạng của mình phụ thuộc vào code có chạy không; công việc ảnh hưởng khá nhiều đến cuộc sống - âu cũng là miếng cơm manh áo mà :) 

Thế nên mình sẽ viết 1 series kể về chuyện nghề, nếu có giỏi hơn tí nữa thì viết về chuyện ngành luôn :)

<!-- more -->

>Đôi ba dòng giới thiệu series đã hết; xin được vào nội dung!!!

![Pair programming](https://spin.atomicobject.com/wp-content/uploads/Pairing.jpg)

Nhìn ảnh này chắc bạn nào làm IT sẽ biết được gọi là Pair programming - 1 người code, 1 người ngồi xem và review; thỉnh thoảng đổi qua lại vị trí cho đỡ buồn.

Liên quan đến cái này là hôm nay anh Tài (người Nhật - tên được Việt hóa cho dễ nhớ); ngồi đối diện mình tự nhiên đứng dậy nói cái gì đấy. Nội dung của cuộc nói chuyện ban đầu như sau.

**A Tài:** #)(*)23$)@#($8)@#($*) 

**B:** Nói tiếng Anh đi anh

**A Tài:** )()2 3$)ca ew a @D@D!S3@4@ **sang đây t nhờ tí** ($8)@#($ 

*Đoán mò nội dung và cũng không nhớ nổi anh Tài nói tiếng Anh hay tiếng Nhật nữa*

Nhân vật phụ vội rút sạc và ôm laptop sang dù không hiểu gì. Đoạn này mình cứ ngờ ngợ; vì đang làm OJT có liên quan đếch gì đến việc khác đâu, chả biết anh ý gọi sang làm gì. Sang đến nơi thì đã thấy anh bày sẵn ghế, chỉ việc ngồi nhìn.

Anh Tài mở ra 1 đoạn document hướng dẫn deploy, copy từng lệnh rồi gõ enter. Thỉnh thoảng mình cũng hỏi mấy câu cho tỏ vẻ hiểu biết :3

**A Tài:** t sẽ clone code từ git về rồi biên dịch bla bla

**B:** À, lệnh này là clone về rồi check hash xem file có nguyên vẹn đúng không?

**B nghĩ thầm** ơ, hay là sếp nào bảo anh ý show cho mình xem để nhanh hiểu hệ thống nhỉ :3

*Đoạn này không hiểu phép thần thánh nào làm a Tài nói dễ nghe hẳn, chắc do mutual understanding.* 

Cuối cùng anh Tài mở 1 phần mềm lên, hẹn giờ cho đoạn code đấy tự chạy hàng tuần; cẩn thận mở lịch lên check xem hẹn giờ có chuẩn không, vào năm 2018 thì nó chạy vào những ngày nào. Đến lúc này thì B cũng đã phát hiện ra là anh mới deploy 1 phần xuất báo cáo.

Xong việc, chuẩn bị xách đít về thì anh Tài mới chốt 1 câu

**Anh Tài:** do deploy chức năng lên bản production nên phải có 2 người cùng làm.

Đúng kiểu phút chót chợt hiểu ra toàn bộ vấn đề. Tư duy kiểu này hay thật, những việc đơn giản nhất như copy từng dòng lệnh kia lại rất dễ bị sai. Điển hình như lúc đọc sách dạy code là mình toàn copy rồi ấn chạy; nhiều khi bị lỗi, tìm chỗ sai còn mất công hơn gấp mấy lần làm từ tốn. 

Lúc về chỗ rồi ngồi nghĩ lung tung mình lại nghĩ đến chuyện mấy bác lái tàu, mỗi lần nhìn đèn tín hiệu hay theo dõi gì thì đều chỉ tay theo để chắc chắn không bỏ sót bước nào.

![Đọc số kiểu lái tàu](https://i.ytimg.com/vi/9LmdUz3rOQU/maxresdefault.jpg)

1 bài học sâu sắc tuy biết lâu rồi những chưa sửa nổi là phải giỏi tiếng hơn nữa...

P/S: cơ mà giỏi hơn tí nữa thì làm sao có cảm giác cả thế giới bùng sáng vào phút chót như thế kia được :v