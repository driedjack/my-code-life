_Những thông tin dưới đây đều là kinh nghiệm cá nhân khi đi phỏng vấn một vài công ty, mình tổng hợp lại rồi chia sẻ, vì là kinh nghiệm cá nhân nên không đúng 100%, nên mọi người xem tham khảo và có cách nhìn nào đó về cấp độ khi phỏng vấn, cần chuẩn bị kiến thức phù hợp và áp dụng được trong lập trình nhá._

## Dấu == trong javascript

```js
1 == '1' // return what?
```
Đáp án là true. Vì sao? Trong javascript, so sáng bằng nhau có hai loại, một loại là so sánh nghiêm (`===`), chỉ trả về true nếu hai toán hạng có cùng kiểu và cùng giá trị, loại còn lại là so sánh bằng bình thường (`==`),  toán tử này sẽ chuyển hai toán hạng về cùng một kiểu (nếu khác kiểu) rồi so sánh (dùng so sánh nghiêm). Khi đó `'1'` sẽ chuyển về cùng kiểu với `1`, `1 === 1` cho kết quả true.

## Class khác Module như thế nào?

Về khác nhau mình nghĩ chắc là sẽ có rất nhiều thứ khác nhau nhưng để chỉ ra cái khác nhau lớn nhất là nằm ở chỗ từ class có thể tạo được đối tượng mang tất cả tính chất được định nghĩa sẵn trong class đó còn module thì không. Ta có thể `include` module vào class, nhưng ngược lại thì không. Ruby dùng đơn kế thừa nên chỉ có duy nhất một class cha (và `Class` được kế thừa từ `Module`) nên module nhằm giúp cho class có thể có thêm nhiều tính chất khác ngoài tính chất kế thừa từ cha.

## Số 1 có phải là object trong Ruby không?

Có, và nó là object của lớp `Integer`. Có thể thử bằng irb
```
>  1.class
=> Integer
>  1.object_id
=> 3
```
Tại sao? Trong Ruby mọi thứ đều là object, đó là triết lý của người sáng tạo ra ngôn ngữ này. Cho nên số 1 cũng là object, id của object này luôn là 3, nó luôn chiếm vùng nhớ xác định, vì 1 là 1 không phải là một giá trị khác, một điểm trỏ đến nào khác.

## Phân biệt update_attribute, update_attributes và update_all

`update_attribute` dùng để update một thuộc tính bất kì của một record nào đó, hàm này bỏ qua validate, vẫn chạy qua các callback bình thường và update luôn cả các thuộc tính đã dirty (có bị chỉnh sửa).

```ruby
person.update_attribute(:adult, true)
```

`update_attributes`, hàm này có vẻ màu mè nhưng thực chất chính là hàm update mình dùng hằng ngày nên khỏi phải nói thêm.

```ruby
person.update_attributes(nickname: 'driedjack', age: 25)
```

`update_all`, hàm này gọi trên nhóm đối tượng (relation) hiện tại, nó sẽ dựng thẳng một câu SQL udpate tất cả những thuộc tính được nêu vì thế nó không khởi tạo bất cứ instance nào cũng như không gọi validate hay callback nhưng giá trị truyền vào vẫn sẽ được type cast và serialize bình thường.

```ruby
People.update_all(awesome: true, parents: 'nothing')
```

Lưu ý, `update_all` chỉ được gọi trên relation, hai hàm còn lại dùng cho instance, còn thằng `update` thì dùng được cho cả hai.

_Bài viết sẽ tiếp tục được cấp nhật cho tới khi nào mình tìm được công việc mới._ 😂
