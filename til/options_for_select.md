# [Rails] Vài ví dụ đơn giản về select_tag và options_for_select

Hôm nay mình làm việc với form và liên quan tới ô chọn đơn giản thôi, kiểu như chọn một vài option với giá trị cho trước sẵn tiện cũng tìm hiểu một ít API. Với input select thì có nhiều helper vì mình cần đơn giản và có thể custom toàn bộ nên lựa chọn là `select_tag` cộng với option tuỳ ý luôn nên dùng `options_for_select` (gọn hơn so với `raw`), ngoài ra còn rất nhiều hàm hỗ trợ cho select helper như `options_from_collection_for_select`, `options_from_collection_for_select`, ...

Với `select_tag` ngoài những option html cơ bản còn hỗ trợ cho ta các option như: `multiple` có thể chọn nhiều option, `disabled` nếu là `true` thì sẽ disable luôn cả ô input, `include_blank` nếu là `true` thì sẽ thêm một option với giá trị rỗng, nếu không được set, thì text cũng rỗng nốt, `prompt` tạo một option rỗng với câu yêu cầu người dùng hãy chọn option, `prompt` khi gọi cũng sẽ thực thi hàm

```ruby
I18n.translate('helpers.select.prompt', default: 'Please select')
```

Hàm `options_for_select` dùng để tạo option cho `select_tag`, nó nhận tham số đầu tiên là giá trị để tạo text và value cho option, có thể dùng array, hash, enumerable (trong doc còn có cả *your type*, mình chưa hiểu lắm), nếu là mảng thì text là giá trị đầu còn value là giá trị cuối, tương tự cho hash, key là text và value là value. Biến thứ hai là `selected`, truyền vào value, nếu đúng thì sẽ chọn option đó khi mới load trang, có thể là mảng nếu select multiple. Một vài ví dụ cho dễ hình dung

```ruby
# Tạo 2 option với value và text giống nhau và là phần tử của mảng
# Giá trị được chọn là *MasterCard*
options_for_select([ "VISA", "MasterCard" ], "MasterCard")
# Tạo 2 option text là *Dollar*, *Kroner*
# Value là *$*, *DKK*
options_for_select([["Dollar", "$"], ["Kroner", "DKK"]])
# Tương tự ...
options_for_select({ "Basic" => "$20", "Plus" => "$40" }, "$40")
```

Ngoài ra ta cũng có thể disable một option nào đó, bằng cách đổi tham số `selected` thành hash với key là `disabled` (chuỗi thì disable một option, mảng thì nhiều), `selected`. Ví dụ

```ruby
options_for_select(["Free", "Basic", "Advanced", "Super Platinum"].to_enum,
                   disabled: ["Advanced", "Super Platinum"])
```

Thêm tấm hình cho có màu sắc ha

![select](https://github.com/driedjack/my-code-life/blob/master/images/select.gif)
