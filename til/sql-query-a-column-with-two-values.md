*WIP*

Dự án mình làm bảng dữ liệu khá là nhiều, mình dùng MySQL adapter cho ứng dụng Rails. Có một bảng để lưu thông tin các công việc, ví dụ như *Quản lý, IT, Lễ tân, ...* Có một bảng khác để lưu thông tin ngôn ngữ yêu cầu của công việc đó (với tiếng Anh là ngôn ngữ mặc định nên sẽ không xét tới), mỗi công việc có thể có nhiều ngôn ngữ yêu cầu. Giữa hai bảng này có một bảng trung gian thuộc về hai bảng đã đề cập. Yêu cầu là lọc ra công việc phù hợp với khoá tìm kiếm, có thể có không hoặc nhiều khoá, nếu không có khoá nào sẽ lấy hết công việc, một khoá ví dụ `tiếng Anh` thì sẽ lấy công việc nào có yêu cầu tiếng Anh, còn nhiều khoá thì lấy những ngôn ngữ yêu cầu **tất cả** ngôn ngữ đó.

Giả sử bảng công việc là **jobs**, ngôn ngữ là **languages**, bản trung gian là **speaks**, speaks có lưu hai cột trỏ tới job và language là **job_id** và **language_id**.

