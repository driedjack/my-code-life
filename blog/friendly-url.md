## Giữa số và chữ

Lại tiếp tục công cuộc làm đẹp cho blog, giờ đến phần đường dẫn. Mình dùng Rails cho blog lên mặc định đường dẫn cho từng bài sẽ có id của bài viết trong đó, ví dụ như `http://www.purinpurin.me/posts/5`. Một đường dẫn như này thì có khá nhiều vấn đề. Đầu tiên, là nhìn vào khá là vô nghĩa với người đọc, người đọc đâu muốn biết post 1 post 2 làm gì, cái họ muốn biết là post đó nói về gì và đường dẫn như thế nào để sau này cứ gõ theo là vào được. Thứ hai, với những con số này, mọi người đều biết blog của mình hiện có bao nhiêu bài viết, mình không muốn như thế, kiểu như vạch áo cho người xem lưng ý. Thứ ba, nhìn đường dẫn có chữ đồ thấy chuyên nghiệp hơn chứ nhở 😎. Kaka.

## Phương pháp

Vấn đề này mình muốn tìm gem để thử, rà rà trên mạng thì ra được gem _friendly_id_ có khá là nhiều sao trên github với các tính năng như _slug history and versioning, i18n, scoped slugs, reserved words, custom slug generators_. Vì blog của mình cũng khá đơn giản và mình thấy nó có hỗ trợ i18n nên quất luôn.

## Sử dụng

Gem này dùng cũng khá đơn giản, tài liệu chi tiết lắm nên chắc dễ dùng với mọi người.

Đầu tiên là cài gem, thêm dòng này vào `Gemfile`

```
gem 'friendly_id'
```

Chạy lệnh `bundle`

Thêm cột `slug` vào bảng mình muốn tạo url dễ nhìn

```
rails g migration AddSlugToPosts slug:uniq
```

Slug ở đây sẽ là chỗ chứa cái tên mình muốn hiển thị trên url, được tạo ra từ đối tượng mà mình muốn, với trường hợp của mình là tên bài đăng. Nó sẽ dùng `transliterate` để convert, lưu ý chỗ này nhé.

Thêm một lệnh để lưu lại lịch sử slug nữa, nếu bạn muốn dùng thì dùng, còn mình thì không nên không tìm hiểu sâu

```
rails generate friendly_id
```

Tiếp theo, `rails db:migrate`

Mình dùng cho post nên sẽ sửa trong model của nó

```ruby
class Post < ApplicationRecord
  extend FriendlyId # extend hay include cũng được
  friendly_id :title, use: :slugged
end
```

Trong controller khi tìm post thì dùng thằng `friendly` để tìm nhé

```ruby
class PostsController < ApplicationController
  def show
    @post = Post.friendly.find(params[:id])
  end
end
```

Vì `params[:id]` của mình lúc này sẽ là thằng slug đó, nên cần dùng hàm friendly.

Rồi thế là xong rồi, cứ thêm sửa xoá bình thường thôi. Với những bài đăng đã có từ trước thì cần phải cập nhật lại slug cho nó dùng lệnh này

```ruby
Post.find_each(&:save)
```

## Vấn đề với tiếng Việt

Vì chữ cái của mình không nằm trong bảng ASCII nên khi translate ra thằng slug sẽ bị mất những chữ cái như _ắ, ự, ơ,..._. May mắn là như lúc đầu mình đã nói gem này có hỗ trợ i18n và dùng hàm `transliterate`. Mình chỉ cần tạo một file yml trong locale và khai báo những chữ thay thế cho chữ bị mất là ok, ví dụ như

```yml
// vi.yml
vi:
  i18n:
    transliterate:
      rule:
        ắ: a
        đ: d
        // more
```

Thế là xong, các bạn thấy đấy bài viết này có url là (không copy cũng viết được) _http://www.purinpurin.me/posts/tao-duong-dan-than-thien-de-doc-voi-nguoi-xem_

Cảm ơn đã đọc bài.

## Nguồn

- [Github](https://github.com/norman/friendly_id)
- [Guide](http://norman.github.io/friendly_id/file.Guide.html)
