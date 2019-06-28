Nay cuối tuần rảnh rỗi nên chia sẻ bài chuyển chữ Nhật thành số và ngược lại. Bài này mình được giao làm sau quá trình training và bắt đầu vào dự án để test xem khả năng của mình như thế nào. Bài này của khách hàng bên Nhật gửi qua để đánh giá coder bên team mình. Nội dung là viết chương trình đổi từ chữ kansuji sang số và ngược lại trong phạm vi từ 0 tới 1068 và dưới 50 dòng code. Mình làm biếng nên viết vào trong class `String` với `Integer` luôn. Ví dụ như này, `'十溝四千三垓二十京三百三十億一万千百十'.to_number` cho ra kết quả là `1000000000400300200000033000011110` và `230456.to_kansuji` cho ra `二十三万四百五十六`.

Lúc bắt tay vô làm thì mình không biết là làm từ chữ ra số dễ hơn hay ngược lại và có thể làm xong một cái rồi từ đó suy ra cái còn lại được không nên mình chơi đại thử làm từ chữ sang số trước. Muốn làm được thì trước hết phải biết tất cả các chữ số bên tiếng Nhật. Mình chia ra ba nhóm, nhóm đầu từ 0 tới 9, tạm gọi là nhóm I.

| 零 |	一 |	二 |	三 |	四 |	五 |	六 |	七 |	八 |	九 |
|----|-----|-------|-------|-------|-------|-------|------|--------|-------|
|0 |	1 |	2 |	3 |	4 |	5 |	6 |	7 |	8 |	9 |

Nhóm thứ hai dành cho số mũ từ 10, 100 và 1000 – nhóm II.

| 十 |	百 |	千 |
|----|-----|-------|
| 10 |	100 | 1000 |

Nhóm cuối cùng dành cho các chữ tương ứng của 10^4 tới 10^68 – nhóm III.

| 万 |	億 |	兆 |	京 |	垓 |	𥝱 |	穣 |	溝 |	澗 |
|----|-----|-------|-------|-------|-------|-------|------|--------|
| 10^4 | 10^8| 10^12 |	10^16 |	10^20 |	10^24 |	10^28 |	10^32 |	10^36 |
| 正 |	載 |	極 |	恒河沙 |	阿僧祇 |	那由他 |	不可思議 | 無量大数 |
| 10^40| 10^44 | 10^48 |10^52|	10^56 |	10^60 |	10^64 |	10^68 |

Lí do mình chia ra như vậy là vì với những số từ 0 tới 9 thì một chữ tương ứng với một số, nếu là 6 thì là 六 và 9 là 九. Còn với 10, 100, 1000 và phần còn lại, chúng đều là phần chỉ vị trí của số trong chuỗi số (ý mình là với 123 thì 百 sẽ là vị trí của 1 trong số, tương tự với 十 cho 2, 3 là chữ số hàng đơn vị nên không cần) thì tại sao lại tách chúng ra làm hai bảng? Vì với 10, 100, 1000 chúng ta không cần phải thêm chữ 一 vào phía trước, ví dụ 10 thì sẽ là 十 không phải 一十, nhưng với các luỹ thừa lớn hơn ta phải thêm 一, ví dụ 10000 phải là 一万.

Bắt đầu tới ý tưởng tính toán, vì kết quả trả về là số nên mình sẽ dùng các phép tính toán để lấy về kết quả cuối cùng. Mình lấy các chữ trong nhóm III làm mốc, cứ mỗi lần tới chữ đó thì tính tổng một lần. Ví dụ như số 230456, số 0 nằm ở vị trí trong nhóm III, nên mình sẽ lấy 23 nhân với 10000 cộng với 456, tương tự như vậy cho những số lớn hơn. Để làm được như vậy mình cần ba biến đựng, a chứa đúng giá trị của chữ số từ 0 tới 9, ví dụ 六 thì a = 6, b để chứa giá trị của các chữ nằm trong phạm vi từ 10 tới 9999, ví dụ 四百五十六 thì b = 456, và sum để trả về tổng cuối cùng là giá trị của chuỗi số đó. Thử với chữ 二十三万四百五十六. 二 là 2 nên a = 2, tiếp tới 十 là 10 thì b = 2 * 10. Tiếp theo là 三 3, a = 3. 万 (10^(4)) thuộc nhóm III, tới đây ta cần tính sum, sum = sum + (b + a) * 10^4 = 230000. Và tương tự như vậy ta có sum cuối cùng sum = 230000 + 456 = 230456.

Rồi vào viết code luôn thôi. Như đã chia ra thì mình cần

```ruby
group_1 = %w(零 一 二 三 四 五 六 七 八 九)
group_2 = %w(十 百 千).unshift('')
group_3 = %w(万 億 兆 京 垓 𥝱 穣 溝 澗 正 載 極 恒 阿 那 不 無).unshift('')
```

`unshift` để chèn khoảng trắng vào đầu array để có index tương ứng với số mũ, ví dụ 百 là 10^2 thì `group_2.index(‘百’) = 2`, tương tự cho group_3. a, b, sum có giá trị ban đầu là 0. Dùng hàm `each` cho mỗi kí tự trong chuỗi chữ.

```ruby
self.each_char.with_index do |char, i|
```

Sum bao giờ cũng cộng dồn với giá trị tổng b và a nhân với 10 mũ vị trí của group_3. Nói về vị trí thì bắt đầu mỗi vòng lặp sẽ tìm xem nếu kí tự đó nằm trong group_2 thì thêm biến `power_10_from_1_to_3` có giá trị là luỹ thừa của 10 với số mũ là vị trí tương ứng.

```ruby
power_10_from_1_to_3 = group_2.include?(char) ? 10**group_2.index(char) : 0
```

Nếu nằm trong group_3 thì cần biến `power_10_from_4_to_68`

```ruby
power_10_from_4_to_68 = group_3.include?(char) ? 10**(group_3.index(char)*4) : 0
```

Như đã nói, ta có

```ruby
sum = (b + a) * power_10_from_4_to_68
```

Vì a có giá trị là chính số đó nên

```ruby
a = group_1.index(char).to_i
```

hàm `to_i` cho trường hợp char không nằm trong group_1 thì sẽ cho a = 0.

Tới lượt b, nếu a bằng 0 thì b sẽ có giá trị cộng dồn thêm `power_10_from_1_to_3`, ngược lại thì a nhân với `power_10_from_1_to_3`. Nếu `power_10_from_4_to_68` khác 0 thì reset b về bằng 0 vì giờ đã chuyển sang pha khác. Và cuối cùng check xem liệu đã là kí tự cuối cùng chưa, nếu phải sum sẽ cộng thêm (b + a).

Xong, code của mình sẽ trông như thế này

```ruby
class String
  def to_number
    group_1 = %w(零 一 二 三 四 五 六 七 八 九)
    group_2 = %w(十 百 千).unshift('')
    group_3 = %w(万 億 兆 京 垓 𥝱 穣 溝 澗 正 載 極 恒 阿 那 不 無).unshift('')
    a = b = sum = 0
    self.each_char.with_index do |char, i|
      power_10_from_4_to_68 = group_3.include?(char) ? 10**(power_4_to_68.index(char)*4) : 0
      power_10_from_1_to_3 = group_2.include?(char) ? 10**power_1_to_3.index(char) : 0
      sum += (b + a) * power_10_from_4_to_68
      a == 0 ? b += power_10_from_1_to_3 : b += a * power_10_from_1_to_3
      b = 0 if power_10_from_4_to_68 != 0
      a = group_1.index(char).to_i
      sum += (b + a) if (i == self.size - 1)
    end
    puts sum
  end
end
```

Chắc là post này tới đây thôi, dài quá rồi, phần ngược lại cho một post khác, cũng là có thêm thời gian cho các bạn rảnh thì làm thử chuyển đổi ngược xem sao, cũng thú vị lắm.
Vì là lần đầu mình chia sẻ thuật toán (nghe oách gớm 😛) mà mình nghĩ ra nên còn lủng củng, mong nhận được góp ý của các bạn.
