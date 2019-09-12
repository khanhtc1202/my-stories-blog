---
title: Chuyện dev số 3 - viết tài liệu có tâm 
date: 2019-09-13 00:27:48
tags: devstories
thumbnail: https://i.imgur.com/K9PCRkM.jpg
---

Ở team mình có sử dụng docker và OpenShift để triển khai các ứng dụng Microservice ; mỗi lần deploy thì sẽ chạy helm để install 1 số thành phần miễn phí (gitlab,mongo….), dùng lệnh của cài đặt của Openshift (oc apply -f abcxyz.yaml) để cài các thành phần khác. 

_Đoạn này có thử chuyển hết về helm chart có vẻ tốt hơn; nhưng mà việc chuẩn bị helm chart hơi phức tạp nên công đoạn này vẫn đang pending lại._

<!-- more -->

Mấy hôm trước mình có làm tài liệu cách cài đặt 1 service và lưu lại tài liệu dạng 1 repository chứa các file yaml và 1 file markdown các bước và lưu lại trên git. Nay có 1 anh khác trong team làm theo thì không chạy được và mình bị triệu hồi vì tài liệu không chuẩn.

Ngồi gỡ rối 1 lúc mới phát hiện ra là sau khi cài xong hết các thành phần thì phải vào giao diện web của OpenShift chỉnh sửa lại 1 vài cài đặt nữa thì mới dùng được. Đoạn này hôm trước vừa sửa vừa thử, chắc lúc chạy được vui quá mà quên mất không thêm vào tài liệu.

Vài bài học rút ra là:
- Viết tài liệu có tâm, luôn luôn ghi lại những gì mình đã làm. Nếu có gặp vấn đề gì đó mà fix được thì nên ghi lại cả lỗi đó, cách tư duy giải quyết vấn đề, nơi tra cứu,…
- Như project của mình thì mỗi 1 thành phần có 1 repository riêng, vậy nên file cài đặt có thể nằm rải rác ở các repository; hướng dẫn cài đặt có thể lại nằm trong file readme đính kèm hoặc là ở wiki của team -> Nên lưu vào 1 nơi thôi (chém gió vĩ mô thì gọi là nguồn chân lý - Single Source of Truth).
- Không to tiếng với đồng đội khi bị hỏi liên tục do tài liệu không update hay bị hỏi quá nhiều thứ đã có sẵn. Lý do chính đa phần là do bạn viết tài liệu quá khó hiểu.
- Deploy hệ thống chạy bằng microservice chưa bao giờ là dễ.
- …

Chuyện nhỏ, nhưng vì mình chủ quan nên suýt thành chuyện "hơi to".

