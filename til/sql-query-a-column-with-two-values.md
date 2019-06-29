Dự án mình làm bảng dữ liệu khá là nhiều, mình dùng MySQL adapter cho ứng dụng Rails. Có một bảng để lưu thông tin các công việc, ví dụ như *Quản lý, IT, Lễ tân, ...* Có một bảng khác để lưu thông tin ngôn ngữ yêu cầu của công việc đó (với tiếng Anh là ngôn ngữ mặc định nên sẽ không xét tới), mỗi công việc có thể có nhiều ngôn ngữ yêu cầu. Giữa hai bảng này có một bảng trung gian thuộc về hai bảng đã đề cập. Yêu cầu là lọc ra công việc phù hợp với khoá tìm kiếm, có thể có không hoặc nhiều khoá, nếu không có khoá nào sẽ lấy hết công việc, một khoá ví dụ `tiếng Anh` thì sẽ lấy công việc nào có yêu cầu tiếng Anh, còn nhiều khoá thì lấy những ngôn ngữ yêu cầu **tất cả** ngôn ngữ đó.

Giả sử bảng công việc là **jobs**, ngôn ngữ là **languages**, bản trung gian là **speakings**, speakings có lưu hai cột trỏ tới job và language là **job_id** và **language_id** với một vài dữ liệu mẫu như sau:

jobs

| id | name          |
|----|---------------|
|  1 | IT            |
|  2 | Manager       |
|  3 | Lái máy bay   |

languages

| id | name              |
|----|-------------------|
|  1 | tiếng Việt        |
|  2 | tiếng Anh         |
|  3 | tiếng Ả rập       |
|  4 | tiếng @$#%^       |

speakings

| id   | job_id | language_id |
|------|--------|-------------|
|    1 |      1 |           1 |
|    2 |      1 |           2 |
|    3 |      1 |           3 |
|    4 |      2 |           4 |
|    5 |      3 |           2 |
|    6 |      3 |           4 |

Người dùng muốn tìm công việc có yêu cầu **tiếng Anh**, khi đó câu SQL sẽ là
```sql
SELECT *
FROM jobs j
INNER JOIN speakings s
ON j.id = s.job_id
WHERE s.language_id = 2;
```

| id | name          | id   | job_id | language_id |
|----|---------------|------|--------|-------------|
|  1 | IT            |    2 |      1 |           2 |
|  3 | Lái máy bay   |    5 |      3 |           2 |

Tiếp họ muốn yêu cầu cả tiếng Anh **và tiếng Ả rập**, mình cứ theo logic ban đầu thì sẽ cần thêm một điều kiện `AND` nữa cho cột `language_id`, câu SQL sẽ như này
```sql
SELECT *
FROM jobs j
INNER JOIN speakings s
ON j.id = s.job_id
WHERE s.language_id = 2
AND s.language_id = 3;
```
Nhưng kết quả không như mong đợi, MySQL không tìm được kết quả nào cả. `WHERE` với `AND` là để check điều kiện trên cùng một hàng có những tiêu chí phù hợp với điều kiện chứ không phải cùng một cột.

Trước hết ta cần lấy được tất cả những hàng nào có ngôn ngữ là tiếng Anh hoặc tiếng Ả rập, trong danh sách đó ta sẽ nhóm lại theo công việc và sẽ lấy công việc nào có số ngôn ngữ lớn hơn hoặc bằng số lượng ngôn ngữ được tìm kiếm lúc ban đầu (ví dụ này là 2 vì có tiếng Anh với tiếng Ả rập). Lệnh SQL sẽ như này
```sql
SELECT j.id, j.name
FROM jobs j
INNER JOIN speakings s
ON j.id = s.job_id
WHERE s.language_id IN (2, 3)
GROUP BY (j.id)
HAVING COUNT(s.language_id) >= 2;
```
Kết quả sẽ là **IT**. Giả sử ta thêm một hàng vào bảng speakings, với công việc *Lái máy bay* yêu cầu thêm ngôn ngữ là *tiếng Ả rập*, chạy lại câu SQL trên sẽ có thêm công việc này vào danh sách kết quả.

Mong là sẽ có ích cho các bạn. 👹
