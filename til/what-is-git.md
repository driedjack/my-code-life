_Một lần nữa kiến thức này mình đúc kết từ sách Pro Git_

Để hiểu Git chỉ cần nắm được 5 tính chất này: lịch sử là ảnh không phải chỉ sự thay đổi, gần như mọi hoạt động đều ở máy của bạn, tính thống nhất, chỉ thêm dữ liệu và ba trạng thái của nó.

## Lịch sử là ảnh không phải chỉ sự thay đổi

Như bài trước mình có nói, mỗi phiên bản là một trang sách - một tấm ảnh phản ánh toàn bộ file của kho tại một thời điểm. Bạn có thể quay trở về bất cứ thời điểm nào với những file và nội dung của đúng thời điểm đó. Không như các VCS khác, mỗi phiên bản chỉ phản ánh sự thay đổi mà thôi.

## Gần như mọi hoạt động đều ở máy của bạn

Git lưu tất cả các phiên bản trong cơ sở dữ liệu trên máy bạn thế nên bạn cứ lấy code từ đó ra mà làm khi bạn mất mạng, đang trên tàu hay máy bay,... những thay đổi của bạn sẽ được lưu lại dưới dữ liệu cục bộ khi có mạng bạn chỉ việc đẩy lên server để mọi người cùng thấy được. Nếu bạn muốn so sáng sự khác nhau của phiên bản hiện tại hay của 5 ngày trước thì cũng chỉ việc lấy ở dưới máy mình ra và so sánh.

Mình rất thích điểm này, mình có thể ở đâu đó và làm việc ngày này qua ngày kia, nào cần thì đem tới chỗ nào có mạng lấy code mới về rồi sửa lại những chỗ đụng nhau cho khớp là xong.

## Tính thống nhất

Mọi thông tin trước khi lưu vào cơ sở dữ liệu đều được kiểm tổng (checksum), và sau này gọi tới cũng chính bằng kiểm tổng này, mọi thông tin nhé. Có nghĩa là git lúc nào cũng biết tất cả các hoạt động của mình trong kho git. Ví dụ, những tấm ảnh thông tin đều được đánh kiểm tổng, nếu bạn muốn đi tới phiên bản nào thì chỉ cần chọn kiểm tổng đó.

Để tính kiểm tổng git dùng cơ chế SHA-1.

## Thường chỉ thêm dữ liệu

Git thêm dữ liệu cho như hành động của bạn vào database, như thế sẽ khó bị đánh mất dữ liệu.

## Ba trạng thái chính của file

- _Đã chỉnh sửa_: có thay đổi trên file nhưng chưa được lưu vào cơ sở dữ liệu
- _Đã đánh dấu_: đánh dấu file thay đổi trong phiên bản hiện tại vào phiên bản tiếp theo
- _Đã xác nhận_: dự liệu đã được lưu an toàn vào cơ sở dữ liệu

Ba trạng thái này dẫn tới ba phần chính của Git là cây làm việc, vùng đánh dấu, và thư mục git. Mình xin được lấy ảnh và dịch từ sách Pro Git ra đễ bạn dễ tham khảo

Theo đó dòng làm việc cơ bản của bạn sẽ như sau:

1. Thay đổi trong cây làm việc
2. Chỉ đánh dấu những thằng cần lưu vào vùng đánh dấu
3. Xác nhận lưu những thay đổi trong vùng đánh dấu vĩnh viễn vào cơ sở dữ liệu

Đó là những điểm cơ bản về git mà mình nghĩ các bạn cần nắm để học và áp dụng những thao tác chức năng khác như `checkout`, `merge`, `reset`, `push`,...
