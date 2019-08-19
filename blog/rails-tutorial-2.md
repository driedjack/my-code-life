Trên log, để ý dòng này

```bash
Processing by Rails::WelcomeController#index as HTML
```

*Xử lý bởi `Rails::WelcomeController#index`(1) như là `HTML`(2).*

Rails log nói cho ta biết hai điều, có thể nhờ nó mà hiểu một phần cơ bản có chế nhận và xử lý yêu cầu của Rails. Với `HTML` (Hypertext Markup Language) là bộ khung của trang web như các bạn đã biết, gọi `localhost:3000` trên thanh địa chỉ là ta đang muốn hiện thị một trang `HTML` trên trình duyệt thế nên Rails mới xử lý request của mình theo kiểu `HTML` đống html đó chính là cái trang hiện lại khi request hoàn tất (mình có chụp lại ở phần trước).

Trong Rails, nơi để xử lý các request là **controller (C)** (đứng trước dấu `#` trong (1)), ở đây nó sẽ gọi một action phụ trách đúng chính xác yêu cầu đó (`index`). Khi ta request về server sẽ có một bộ phận phụ trách đọc đường dẫn để biết controller với action nào sẽ xử lý request này rồi bàn giao nhiệm vụ cho nó, nếu request cần trả về `HTML`, bộ phận nhắc trước đó sẽ tìm đúng đoạn mã chịu trách nhiệm render `html` ấy - trong Rails gọi là **views (V)** - để server trả về cho trình duyệt.

Ngoài ra, còn có resource. Với những ứng dụng dùng dữ liệu động như người dùng, sản phẩm,... thì còn một lớp khác chịu trách nhiệm đại diện cho dữ liệu của chúng trong database nữa, chúng được gọi là **model (M)**.

3 chữ mình tô đậm đại diện cho mô hình thiết kế của Rails, MVC. Mình nghĩ các bạn cần nắm rõ điều này, vì nó là cốt lõi, khung sườn của Rails, vì thế có một vài quy ước bạn cần tuân theo mà qua đây mình cũng nói luôn để bạn có một cách nhìn tổng quan (Mà hình như nói ở trên rồi 🤣).

![mvc](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/MVC-Process.svg/1920px-MVC-Process.svg.png)

---

Thôi thì giờ bắt tay vô thử luôn nhé. Trước hết ta cần một thằng làm giùm CSS để có thể tập trung vào Rails hết, thế nên cùng cái Bootstrap cho tiện nhé, vì thằng này nổi tiếng quá rồi.

Thêm dòng này vào cuối file `Gemfile`[^1]

```ruby
gem 'bootstrap'
```

Trong terminal chạy lệnh `bundle`[^2]. Tắt (`Control (⌃) + C` (nên nhớ nhé, dùng hoài đó)).

```Sửa file `app/assets/stylesheets/application.css`[^3] thành đuôi `.scss`[^4]

```bash
mv app/assets/stylesheets/application.css app/assets/stylesheets/application.scss
```

Gọi bootstrap trong file này bằng cách thêm dòng

```
@import "bootstrap";
```

Sau đó xoá dòng ` *= require_tree .`

Rồi xong rồi đó, bật server lên lại và chuẩn bị chiến thôi.

---

Khi vào đường dẫn của một trang web thì sẽ vô gì trước nhỉ, có phải là một trang mở màn thật hoành tráng để người ta biết mình là gì đúng không nào. Thế thì ta hãy tạo trang này nhá.

Để người dùng vô được trang mình, cần cung cấp người ta một đường dẫn. Việc xử lý tạo đường dẫn, luân chuyển request tới đúng nơi được do bên trong thằng Rails xử lý, nó cung cấp bề nổi cho ta file `config/routes.rb` để khai báo đường dẫn trong đó. Trang chủ chính là trang mà khi gõ đường dẫn gốc vào thì mặc định đi tới trang này.

```diff
Rails.application.routes.draw do
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html+
+ root 'general#home'
end
```

Ở đây ta nói với Rails là action `home` của controller `general` sẽ xử lý request của đường dẫn gốc (`root`). Cú pháp là

```ruby
root 'controller#action'
```

Ta tạo file controller `app/controllers/general_controller.rb` với nội dung

```ruby
class GeneralController < ApplicationController
  def home; end
end
```

Với action `home` chính là một hàm Ruby. Tiếp đến ta cần một view để render html, tạo file `app/views/general/home.html.erb` với nội dung

```erb
<h1> Xin chào </h1>
```

Vào `localhost:3000` xem nào. Bingo, chữ *Xin chào* hiện lên là thành cmn công rồi nhá!
