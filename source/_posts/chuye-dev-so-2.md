---
title: Chuyện dev số 2 - SSL
date: 2017-12-09 10:48:31
tags: devstories
thumbnail: https://www.pensacolawebdesigns.com/wp-content/uploads/2017/08/ssl.png
---

Khi truy cập vào mấy trang có nội dung nguy hiểm thì nhiều khả năng sẽ thấy 1 trang đỏ quạch thế này xuất hiện.

![Thông báo đẫm máu](https://media.gcflearnfree.org/content/55e076c07dd48174331f528f_01_17_2014/privacy_malware_page.png)

Nếu vì 1 nội dung nào đó vẫn kiên quyết muốn truy cập thì chỉ cần ấn "Detail" rồi chọn chọn lung tung 1 hồi cũng sẽ vô được. Tất nhiên khi đã truy cập vào những trang thế này thì có khá nhiều rủi ro tiềm ẩn có thể sẽ gặp phải rồi.

*Ngày xưa mình cũng hay cố vào mấy trang kiểu vậy lắm, giờ thì thôi tắt luôn :3*

Nguyên nhân chính do SSL certificate. Khi có SSL certificate sẽ dùng được https - giúp dữ liệu truyền/nhận được bảo mật, và thông qua certificate có thể xác thực được là mình đang vô đúng trang web của cá nhân/tổ chức đó luôn.

<!-- more -->

Gần đây mình có làm việc xung quanh vấn đề này, có vài chuyện nhỏ vui vui mình viết thành mấy mục cho dễ đọc.

## Đừng tin quá nhiều vào mentor

Mình có 1 anh mentor, xin được phép gọi thân thương là anh Su :3

Anh Su cũng là người ngoại quốc, có 1 thời gian làm việc dài dài bên Huê Kỳ. Anh học tiếng Nhật qua anime và nghe nói giỏi hơn mình (ấy là anh mới sang Nhật được 7 tháng thôi đó). Chỉ có điều do tự học nên anh không đọc được Kanji mấy.

1 số tool nhỏ của team có dùng VPS của Azure, tất nhiên đã block port để chỉ có ip từ mạng công ty mới truy nhập được. Lúc mình mới bắt đầu làm thì thấy lúc truy cập vô mấy trang đó đều có https, nhưng mà https sẽ bị thế này:

![Https đểu](http://imh01-inmotionhosting1.netdna-ssl.com/support/images/stories/website/errors/ssl/chrome-your-connection-is-not-private.png)

Mình có hỏi thì anh Su bảo là do mấy cái này dùng self-signed ceritificate nên sẽ bị như thế. Mấy cái này dùng nội bộ nên không muốn bỏ tiền, mà xin approve certificate từ công ty cũng lâu lắm; thế nên dùng cái này cho nhanh. Mình đang làm 1 tool của team cũng chưa có https và được anh Su cho cái link cách tạo self-signed certificate luôn.

Lúc đầu mình cũng chỉ biết thế, mấy hôm sau mới nhớ ra là giờ có bọn [Let's Encrypt](https://letsencrypt.org/) cấp ssl free mà. Thế là lên công ty bảo anh Su:

**B:** sao mấy cái site này không dùng Let's Encrypt anh ơi. Em thấy nó free mà.

**Anh Su:** tao thử rồi, nhưng mà không được. Tao search thì tại mình dùng tên miền free của Azure dạng *.cloudapp.azure.com nên không dùng được đâu.

Tự thấy có gì sai sai, không liên quan lắm. Chả lẽ Azure có certificate cho tất cả subdomain của nó rồi nên không cài được??? Mà cái Wildcard SSL Certificates hình như siêu đắt luôn. Cơ mà lúc đấy cũng chưa nghĩ ra, vài ngày sau mới nói với anh Su.

**B:** anh ơi cho em dùng thử Let's Encrypt trên cái site em đang làm nhé.

**Anh Su:** ừ cứ làm đi, có được thì bảo tao cách làm nhé.

*15 phút sau*

**B:** hơ hơ chạy được rồi này anh ơi. Https xịn A+ grade luôn :))

Và sau đó mình chỉ lại anh Su cách dùng Let's Encrypt rồi cùng ngồi cài lại lên mấy cái site còn lại của team.

Hết chuyện đầu tiên. 

*Cái site nho nhỏ mình đang làm có mở port với thế giới, thế nên bị  [Google safe browsing](https://safebrowsing.google.com/) liệt vô dạng nguy hiểm vì có trang đăng nhập mà không có https. Mấy browser khác như firefox, safari,... cũng dùng data này của Google luôn, thế nên site bị màn hình đẫm máu ở mọi nơi. Đoạn này chỉ cần cài https rồi theo hướng dẫn của Google sau là ổn.*

## Tìm hiểu kỹ trước khi bắt tay vào làm

Với Let's Encrypt mình có dùng certbot để tạo certificate. Certbot này cũng khá xịn, tự động config lại với mấy webserver thông dụng luôn.

![Người hùng cổng 443](https://certbot.eff.org/images/certbot-OG.png)

Khi chạy certbot thì cần mở cổng 80 hoặc 443, đại thể để nó xác thực với Let's Encrypt về quyền sở hữu tên miền này, và sau nó sẽ cấp cho SSL certificate.

Do là hàng free nên certificate này có 90 ngày là hết hạn, phải chạy lại certbot để nó gia hạn cho 90 ngày nữa. Cái này có thể dùng crontab là xử lý được; theo như tutorials trên mạng thì cứ crontab chạy hàng ngày, khi nào mà certificate còn dưới 30 ngày thì nó sẽ gia hạn cho.

Đến đây thì có 1 vấn đề là mấy tool kia toàn hàng nội bộ và được block port với thế giới thì làm thế nào mà nó gia hạn được đây :s

Thế là mình lại ngồi bàn với anh Su tiếp. Anh ý bảo xem thử Azure CLI coi, có thể làm được. Mà nó có SDK cho Java luôn đó.

Vậy là mình tưởng tượng ra quy trình có mấy bước thế này:

1. Hẹn lịch chạy renew. Mấy cái này cần bảo mật nên mở càng ít càng tốt, cỡ 20 ngày chạy 1 lần chắc ổn...
2. Mở port 443
3. Chạy certbot để gia hạn
4. Đóng port 443

Bước số 2 + 4 thì Azure CLI và SDK cho Java đều làm tốt. Cái số 3 thì Azure CLI có run-command để chạy lệnh trực tiếp trong VPS luôn. Mỗi tội nó được đánh dấu Preview, và trong Java SDK thì chưa có cái này => Trong java gọi shell command để chạy Azure CLI là ổn.

Mỗi tội đời không như mơ, mỗi lần gọi run-command chỉ chạy được 1 command và phải chờ nó xong thì mới gọi lệnh khác được. Mình nghịch lung tung bằng ls, cp,... 1 hồi thì nó đơ luôn; không sao chạy qua run-command của Azure CLI được nữa. Đúng là hàng preview có khác :s

Cũng chưa phải vấn đề lớn lắm, bước 3 không dùng Azure CLI nữa. Java có nhiều thư viện có thể ssh vào VPS được luôn, anh Su bảo mình dùng [sshj](https://github.com/hierynomus/sshj). Cái này thì chả có document gì luôn, được cái có thể hỏi anh Su cách dùng nên cũng không vấn đề gì lắm.

Tèn tèn, công cụ cũng đã tạm đủ. Bắt tay vào dev.

...

...

...

Cơ mà quái, sao đóng port rồi mà chạy renew vẫn thành công nhỉ :s

Có gì đấy sai sai.

Đến lúc này mới nhận ra là certbot không cần kết nối với server của nó vẫn chạy gia hạn được. ~~Mình đoán là nó chỉ cần chứng thực lại và sửa ngày hết hạn trong certificate đã có sẵn thôi; không cần phải giao tiếp với server nữa. Thế nên cái chuyện đóng, mở port kia chả còn gì quan trọng. Vấn đề được giải quyết bằng 1 lệnh crontab đơn giản.~~ 

*Thông tin đính chính ở phía dưới*

Vẽ ra lòng vòng 1 hồi, cũng phải mất mấy ngày mà cuối cùng lại chết vì thiếu 1 bước... Đành tự nhủ lòng là thôi cũng biết Azure CLI nó tròn méo thế nào ;(

Báo với anh Su, và rất may là mình cũng hay thảo luận với anh ý trong lúc làm. Thế nên anh ý cũng không mắng gì cả :3 Cơ mà kiểu ăn hại mấy ngày nên hơi cắn rứt ;(

>Update sau 1 tuần tìm hiểu: thật ra là vẫn cần mở cổng. Quá trình này nó gọi là domain challenge (hay domain authorization); cái này có cache 30 ngày nên lúc mình thử renew lại không cần mở port vẫn chạy được. Chi tiết có thể xem thêm trong [FAQ](https://letsencrypt.org/docs/faq/).

*Mấy ngày trước mình có đọc đâu đó là style của Mỹ là vừa làm vừa rút kinh nghiệm thoải mái. Còn style của Nhật là sẽ lên kế hoạch kỹ càng trước khi làm, thế nên nếu anh Su là người Nhật thì rất có thể đã bị mắng tơi tả rồi. Btw, đoạn này không nhớ rõ lắm, phải kiếm sách đọc lại đã.*


Thật ra mấy cái chuyện cài server, ssl, mua domain,... ngày trước do không có điều kiện tiếp xúc nên mình thấy cao siêu lắm. Giờ có thể mua được domain, cài ssl rồi mới thấy nó cực kỳ đơn giản. Các bạn có thể nghịch thử cho biết; như Azure Cloud thì free 200$ trong vòng 1 tháng cho thành viên mới đăng ký - điều kiện là phải nhập thông tin thẻ thanh toán quốc tế.

<p align="center"><b>The end.</b></p>