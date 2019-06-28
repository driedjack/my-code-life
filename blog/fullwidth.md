## Mục lục

- Vấn đề
- Phân tích
- Tìm hiểu
- Code

## Vấn đề

Nghe tên bài là thấy chuối rồi phải không? Lúc bắt đầu ý tưởng viết về vấn đề này thì thật sự trong đầu chỉ hình dung được làm sao để chuyển số gõ bằng bàn phím tiếng Nhật về lại số khi gõ bàn phím thường chứ trong đầu cũng chưa định hình được là hai kí tự đó là như thế nào. Giờ rảnh rỗi nên tìm hiểu rồi chia sẻ với mọi người, có thể tới cuối bài viết mình sẽ biết phải gọi tên như thế nào nhưng giờ cứ dùng cách gọi này để thể hiện việc lúc bắt đầu mình cũng ngu ngơ phết đi. 😎

Hãy thử cuộn trang lên và bôi đen hai số 3 của tên bài viết này xem nào. Wow, dải bôi đen bên trái rộng hơn bên phải đúng không? (và có lẽ là gấp đôi đấy).Lạ nhỉ! Giờ nếu bạn có một ô input có kiểu là số, thử chép hai số 3 bên trên vào và submit thử xem có gì lạ? Số 3 bên trái bị báo lỗi không phải là số đúng không? Hai điều lạ!

![3](https://i.gyazo.com/2e35ed0031f88f0254c8bb53872aa0ff.png)
![submit](https://i.gyazo.com/585aef78a8bb2dd5fd27196588355fd3.png)

Vậy làm sao có thể giữ nguyên kiểu bàn phím tiếng Nhật để nhập số vào ô input và ActiveRecord có thể validate được thuộc tính before này là số mà không cần phải chuyển về bàn phím thường?

## Phân tích

Ok, hướng đi của mình là sẽ cho ô input này về kiểu chuỗi bình thường bởi vì khi là kiểu số Rails sẽ không nhận những giá trị nào không phải kiểu số để request về server, sau đó cứ mỗi lần submit thì chạy một đoạn script chuyển đổi giá trị về kiểu kí tự bình thường (bắt trên nút submit vì nó là chốt chặn cuối trên màn hình, sau khi ấn thì không làm gì trên màn hình ảnh hưởng tới request nữa).

## Tìm hiểu

Cách làm là thế, bây giờ mình phải tìm hiểu tại sao lại cùng một số 3 mà lại có hai kí tự khác nhau như vậy. Dựa vào kích cỡ kí tự, mình tìm hiểu thì được biết các kí tự CJK (Chinese, Japanese, Korean) được xếp vào kí tự fullwidth và halfwidth, với 2 số 3 khi nãy thì số 3 bên trái là kí tự fullwidth, bên phải là halfwidth. Muốn test thử thì cứ lên google fonts chọn font mono rồi nhập vào thì thấy ngay là kí tự halfwidth đúng bằng một nửa kí tự fullwidth. Halfwidth và fullwidth cũng được đặt tên là U+FF00–FFEF trong Unicode block. Chỗ này cho mình một gợi ý, nếu những kí tự fullwidth đều được mã hoá như này thì dựa vào mẫu hình này có thể map từng kí tự số của fullwidth tương ứng với halfwidth không? Nếu vậy cần phải biết đích xác mã cho từng kí tự số.

Rất may là [wiki](https://en.wikipedia.org/wiki/Halfwidth_and_fullwidth_forms#Block) có list ra hết cho mình. Theo như đó thì mình chỉ cần đổi các kí tự từ U+FF01 tới U+FF5E là ổn. Thế còn các kí tự halfwidth tương ứng thì sao? Chúng đều nằm trong [Basic Latin Unicode block](https://en.wikipedia.org/wiki/Basic_Latin_(Unicode_block)) và có trình tự giống với fullwidth bắt đầu từ U+0021 tới U+007E.

Tất cả các kí tự kể trên đều có thể đưa về code point trong phạm vi UTF-16. Vì chúng được sắp theo đúng thứ tự nên 2 kí tự có khoảng code point bằng nhau và là 65248 (`'!'.charCodeAt(0) - '！'.charCodeAt(0) == 65248`). Cho nên cách code của mình sẽ là:

## Code

1. Bắt sự kiện click trên nút submit
2. Lấy hết những ô input cần chuyển đổi cho chạy vòng lặp
3. Check xem giá trị của ô input có phải là số hay không nếu phải không làm gì cả
4. Nếu không sẽ thay thế mỗi kí tự fullwidth với halfwidth tương ứng bằng cách
5. Lấy code point kí tự cần chuyển trừ 65248 rồi chuyển từ code point đó về lại string.
6. Gán kí tự đó vào giá trị của input.

```coffee
document.addEventListener 'turbolinks:load, ->
  $('#submit_resource').click () ->
    $.each $('.number_input'), (i, input) ->
      num = $(input).val()
      if num and typeof num != 'number'
        converted = num.replace /[\uff01-\uff5e]/g, (match) ->
          String.fromCharCode match.charCodeAt(0) - 0xfee0
        $(input).val(converted)
```

Note một tí về đoạn regex `/[\uff01-uff5e]/g`, `//` để biểu thị đây là regex, `[]` để lấy đúng từng kí tự khớp với mẫu, `\` để biểu thị kí tự đặc biệt, `-` biểu thị khoảng. Cho nên đoạn này có nghĩa là tìm bất kì kí tự nào nằm trong khoảng từ `！` tới `～`.

Rất vui được chia sẻ với mọi người. 

Nguồn tham khảo:
- [Wiki](https://en.wikipedia.org/wiki/Basic_Latin_(Unicode_block))
- MDN: replace(), fromCharCode(), charCodeAt(), regular expressions
