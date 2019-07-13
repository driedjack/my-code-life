Git được giới thiệu trên trang chủ của nó là một **hệ điều khiển phiên bản (VCS) phân bố** miễn phí với mã nguồn mở, xử lý tốc độ và hiệu quả mọi thứ từ các dự án nhỏ cho tới rất lớn. Vậy thì VCS là gì?

Version Control là hệ thống ghi lại những thay đổi của một hay nhiều file qua thời gian để sau này bạn có thể gọi lại tại bất kì một thời điểm cụ thể nào. Có thể xem một đống file của bạn là một quyển sách, mỗi trang sách là một lát cắt thời gian những file của bạn, VC sẽ lưu lại từng trang sách cho từng thời điểm, lúc nào bạn cũng có thể lật giở những trang sách đó để xem lại.

Có 3 loại VCS, loại thứ nhất là *hệ điều khiển phiên bản cục bộ (LVCS)*. Loại này là loại đơn giản ngày xưa hay dùng, đó là lúc nào cần lưu lại dùng sau thì cứ copy hết file vào một thư mục khác được đặt tên theo thời gian hay gì gì đó để phân biệt. Kiểu này thì khá là tốn công, vì thao tác bằng tay hết nên dễ phát sinh lỗi nữa.

Loại thứ 2 là *hệ điều khiển tập quyền (CVCS)* dùng cho những nhà phát triển làm chung nhóm. Lúc này sẽ có một server backup toàn bộ các file, ai muốn dùng gì thì cứ chép về mà dùng. Loại này được cái là ai tham gia cũng đều nắm được mức độ nào đó tình hình công việc của người khác, quản trị viên cũng nắm rõ thông tin và điều khiển mọi việc dễ dàng hơn. Nhưng có một điểm yếu lớn đó là khi cái server này tèo thì mấy anh tham gia cũng đếch làm được gì luôn, còn lỡ mà nó mất hết file là xác định thất nghiệp cả đám.

Dẫn tới loại thứ 3 đó là *hệ điều khiển phiên bản phân bố (DVCS)*. Người dùng lúc này không chỉ có thể lấy các file mới nhất mà họ còn ánh xạ được tất cả các file hay version nằm trên server. Thế nên server mà có tèo thì mấy ổng vẫn làm việc bình thường  và còn có thể đẩy kho file này lên trên server để nó lưu lại nữa.

Và Git là một DVCS!

Ngoài ra, các DVCS còn có thể thể làm việc với các dự án từ xa, có thể đẩy lên đâu đó để người khác lấy về dùng, cùng chung một dự án, biết hết các phiên bản. Dẫn đến một ứng dụng khác rất hữu ích để làm việc với git, đó là Github!

Rảnh thì mình sẽ nói tiếp về github sau nhé!

---

Sẵn mình xin hướng dẫn cài git cho macOs luôn. Mà thôi cái này thì nhiều quá rồi, mình xin lấy bài của *Michael Galarnyk* (dù chưa xin phép, nhưng xin phép dẫn nguồn ở đây!). Các bạn vào [trang này](https://hackernoon.com/install-git-on-mac-a884f0c9d32c) và làm theo hướng dẫn, có ảnh ọt nhìn cho dễ.
