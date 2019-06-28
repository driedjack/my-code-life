Với Rails app có dùng turborlinks, bạn đã bao giờ thử tạo và đi tới link anchor chưa? Vừa qua mình dùng thử để tạo mục lục cho từng bài viết thì gặp vấn đề, cứ mỗi lần nhấp vào anchor link thì browser lại request lên server. Điều mình muốn là chỉ cần trỏ tới một vị trí trong trang, nên như vậy thì rất là tốn tài nguyên.

Qua tìm hiểu thì biết được là trong quá trình nhấp vào link đó turborlinks đã tham gia đón đầu hành động này, nó chen vào giữa và thực hiện một request tới server. Trên github turborlinks thì có hẳn một [issue](https://github.com/turbolinks/turbolinks/issues/75) về vấn đề này nhưng tới nay vẫn chưa được merge vào master.

Trên đó cũng có nhiều mánh khoé để vượt qua tính năng này, nhưng mình chọn cách [tắt turborlinks trên link](https://github.com/turbolinks/turbolinks#disabling-turbolinks-on-specific-links) anchor mình muốn di chuyển.

Mong là có ích cho ai đó cũng muốn làm điều tương tự mình. 🐙
