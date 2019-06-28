*Có những trường hợp ta có số liệu thời gian là một số có đơn vị thời gian để thuận tiện tính toán trong các bài toán phân tích, thống kê như tính tổng, trung bình thời gian, … Nhưng khi dùng thể hiện lên màn hình mà đích đến là người dùng thì cần phải định dạng lượng thời gian đó theo kiểu mà người dùng quen hình dung về thời gian, ví dụ như 95:05:27 là 95 tiếng 5 phút 27 giây chẳng hạn. Bài viết hôm nay là để nói về việc chuyển từ số giây sang định dạng này cộng với việc tại sao lại viết về nó.*

Ý tưởng viết bài này đến từ thực tế trong công việc, muốn hiển thị trực quan thời gian lên màn hình thời gian người dùng trên trang web. Với đầu vào là số giây. Thì khi gặp task mới, như một thói quen của lính mới, mình mò lên google để tìm cách làm. Một vài bài thảo luận trên *Stackoverflow* có nói về vấn đề này, cách mà họ dùng là tạo một đối tượng thuộc lớp `Time` với thời gian từ số giây của mình. Nhưng hạn chế của nó là chỉ đúng với thời gian trong một ngày, nếu số giây lớn hơn số giây của một ngày thì số giờ của ngày bị lố đó bị cắt mất.

```ruby
Time.at(seconds).utc.strftime('%H:%M:%S')

# Với seconds từ 0 tới (86400 - 1) thì kết quả đúng
# --> 53244s: "14:47:24"
# Với seconds lớn hơn giá trị một ngày thì không đúng như mong muốn
# --> 200000s: "07:33:20" (thay vì "55:33:20")
```

Thế nên mình dao kéo lại theo kiểu nếu số giây lớn hơn một ngày thì sẽ lấy số giờ trội cộng với số giờ theo hàm cũ tính từ số giây sau khi đã trừ ra số giờ trội ấy, sau đó ghép lại với đuôi phút và giây.

```ruby
def hms_style(seconds)
  hms  = Time.at(seconds).utc.strftime('%H:%M:%S')
  days = seconds / Constant::SECONDS_IN_DAY
  return hms if days.zero?

  h, m, s = hms.split(':')
  "#{h.to_i + days * 24}:#{m}:#{s}"
end

# hms_style(200000) --> "55:33:20" Bingo!

```

Yeah, thế là xong, đã hoàn thành một chức năng đơn giản, push code và chờ review thôi.

...

Một ngày làm việc trôi qua, tối về cái suy nghĩ về chức năng đơn giản ấy vẩn vơ trong đầu. Tại sao khi ập vô làm mình không chịu suy nghĩ về nó trước mà lại bay đi hỏi người khác như thế. Thế thì mình có tư duy được chút nào không, có học được cách giải quyết vấn đề không, hay chỉ học được cách ăn cắp và đạo nhái ý tưởng. Thế là mình quyết định ngồi xuống và suy nghĩ lại về nó.

Mình cảm nhận đây đơn thuần chỉ là xử lý số và chuyển về kiểu chữ. Dùng tới lớp `Time`﻿ chỉ là một bước dùng sẵn những thứ có vẻ là đúng thôi, với mình cũng chả rõ bên dưới cái hàm đó thực hiện những gì nữa, liệu làm như thế performance có ổn không và tại sao lại liên quan đến lớp `Time` cơ chứ.

Như này, với số giây đó, mình có tính ra được phút với giờ không. Được, bằng cách chia nó cho 60 (số giây của một phút) được phần nguyên của thương là số phút, phần dư ra chính là số giây. Số giây không cần phải xử lý nữa nên sẽ là phần giây của kết quả. Số phút đó có thể lớn hơn 60, vậy thì ta chia tiếp cho 60 (số phút của một giờ) tương tự như trước để có được số giờ và số phút trồi ra. Giờ và phút này chính là kết quả mình muốn.

```ruby
def hms_style(sec)
  min, s = sec.divmod 60
  h, m = min.divmod 60

  "#{two_digits(h)}:#{two_digits(m)}:#{two_digits(s)}"
end

private

# Chuyển từ một chữ số sang hai chữ số
def two_digits(number)
  '%02i' % number
end
```
Yeah, đơn giản hơn rồi. Mình đã có thể tự suy nghĩ và tự giải quyết bài toán rồi.
Thêm nữa, khi dùng benchmark để đo thì hàm mới này nhanh hơn hàm cũ rất nhiều. Ahihi. 🤩

![benchmark](https://raw.githubusercontent.com/driedjack/driedjack.github.io/master/assets/images/hms-bench.png)

Cám ơn đã đọc bài của mình! 😍
