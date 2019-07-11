Xin chào các bạn! Đây là bài đầu tiên trong series giới thiệu về Ruby on Rails dành cho người mới muốn tìm hiểu hoặc thử dùng nó. Series này có đề cập thêm tới những vấn đề cơ bản của lập trình web, ngôn ngữ lập trình Ruby, những thư viện, tính năng cần thiết của Ruby on Rails,... tất cả chỉ nhằm giúp bạn nắm được cơ bản Rails là gì, sẽ làm được gì và như thế nào. Với phương châm cho người đọc cái nhìn đơn giản nhưng thông suốt những vấn đề chính nhất nên mình sẽ trình bày theo kiểu vừa học vừa làm những chức năng thông dụng thường có ở những website, bởi nếu học mà không hành sẽ không nhớ lâu được. 🤓

## Giới thiệu Ruby on Rails

Ruby on Rails (RoR) là gì? Ngọc ruby trên đường tàu, hừm, viên ruby đưa chúng ta ra toàn thế giới bằng tàu lửa? Cũng đúng một phần đấy, RoR là web framework viết bằng ngôn ngữ lập trình Ruby, nó giúp ta tạo ra một cái web và đem ra ánh sáng nhân loại khá là nhanh, còn cái ý mình nói vui vui về tàu lửa thì sau này sẽ rõ 🤭.

### Ruby
Ngôn ngữ lập trình chắc là bạn cũng đã biết, đó là tập hợp những câu lệnh theo cú pháp nào đó mà con người dùng để giao tiếp với máy tính, với ngôn ngữ lập trình ta có thể ra lệnh cho nó làm một vài tác vụ nào đó, ví dụ ta có thể bảo máy tính tính cho ta một phép toán dùng ngôn ngữ Ruby, 12 trừ 3 nhân 4. Ta dùng **irb** để truyền đạt câu lệnh

```bash
>  12 - 3 * 4
=> 0
```

Đoạn lệnh Ruby trên sẽ được dịch về mã máy, máy tính thực thi và trả về đáp án là **0** cho chúng ta. Ta sẽ dùng chính ngôn ngữ này để giao tiếp với máy tính khi lập trình RoR, cụ thể ở đây là server ứng dụng, nói cho nó hiểu những gì ta muốn làm.

Ruby là ngôn ngữ được tạo ra để làm cho các lập trình viên hạnh phúc hơn, cú pháp của Ruby khá đơn giản, dễ đọc dễ hiểu, không rườm rà, theo kiểu tiếng người thông thường nên khá dễ để học và làm việc nhóm, ví dụ như `'Code Ruby vui lắm'.start_with? 'Code'`, bạn có chút ý niệm gì về đoạn này không, có vẻ như là muốn hỏi cái chuỗi gì đó có bắt đầu bằng cái chữ gì đó không đúng không? 😉

### Web framework
Thế còn web framework là gì? Nó là một tập hợp của những khuôn mẫu, mô hình trừu tượng được xây dựng cho người dùng có thể dựa vào để viết và triển khai các ứng dụng web. RoR sẽ cung cấp sẵn một mẫu hình, phương thức chung mà chỉ cần dựa theo đó ta có thể viết được một ứng dụng web có thể được mở rộng, tuỳ chỉnh, dùng lại nó theo mục đích của từng người. Nói chung là ta cứ dựa theo những quy ước của nó là sẽ xây dựng được một cái web ra trò.

### RoR web framework
Với RoR, quy ước có khá nhiều, nhờ vậy nên có nhiều thứ đã có sẵn mà ta không cần làm thêm gì nữa cho nên giang hồ cũng hay nói vui là RoR có nhiều bùa phép, việc hiểu những bùa phép đó cũng là một điểm thú vị để ta đào sâu hơn nữa. Như đã nói, vì viết bằng Ruby nên việc lập trình RoR khá thoải mái, đơn giản (ban đầu thôi) và đặc biệt là nhanh. Vừa thừa hưởng điểm mạnh, nó cũng thừa hưởng luôn yếu điểm của Ruby là tốc độ xử lý chậm, điều này cũng giải đáp luôn cho cái định nghĩa mình nêu ra ở trên, vì chạy bằng tàu lửa nên không nhanh bằng phi cơ! 🙄

## Giới thiệu cách học

Trong series này chúng mình sẽ cùng nhau viết một ứng dụng chia sẻ sách, sẽ có người dùng, có sách, người dùng sẽ đăng tải thư viện sách của mình lên, viết cảm nghĩ hoặc giới thiệu về nó, những người dùng khác có thể xem và trò chuyện về cuốn sách đó. Một ứng dụng với những chức năng đơn thuần thôi đúng không nào.

Với mỗi tính năng hay kiến thức gì đó mới mẻ, mình sẽ cố gắng giải thích theo cách hiểu của mình, ở cuối bài mình sẽ dẫn link cho các bạn tìm hiểu thêm nếu vẫn chưa hiểu hoặc chưa thoả mãn lắm về vấn đề đó. Dù gì thì tự mình dạy mình học là điều dễ chịu và dễ chấp nhận nhất mà.

## Những thứ cần thiết

Vì mình code trên macbook nên nếu các bạn dùng macbook hay hệ điều hành kiểu Unix thì sẽ theo dõi dễ hơn Windows. Với các bạn dùng windows mình khuyến khích các bạn dùng Ubuntu, vì Windows code RoR lỗi hơi nhiều.😅 Về phần cài đặt mình hướng dẫn cho macbook ở đây thôi nhá.

Đầu tiên, bạn cần có một trình soạn thảo để viết code, có nhiều trình soạn thảo ngoài kia ví dụ như Text Mate, Atom, Visual Code, hay Vim, mỗi cái có một cái hay riêng, còn mình thì dùng Sublime Text 3, tại mới vào nghề dùng nó tới giờ quen tay rồi, nó khá là đơn giản, mình là người đơn giản nên cứ dùng nó thôi. Tiếp theo, bạn cần cài Ruby, RoR.

## Cài đặt

Bạn có thể vào trang chủ của Ruby và tải bộ cài về máy cho từng phiên bản Ruby, nhưng làm vậy khá là rắc rối khi bạn làm việc với nhiều dự án với các phiên bản khác nhau, phải chuyển qua chuyển lại linh tinh nên mình khuyến khích các bạn dùng trình quản lý phiên bản Ruby. Có hai trình quản lý thông dụng dùng cho Ruby là rvm và rbenv, nó sẽ quản lý các phiên bản rbenv và cung cấp cho mình vài câu lệnh để chuyển đổi phiên bản hay cài đặt phiên bản nhanh và thoải mái. Mình dùng rbenv nên hướng dẫn cách cài nó ở đây luôn.

Ta cần cài Ruby 2.6.3, Rails 6.0.0.rc1 với cả nodejs nữa. Cài mấy phiên bản mới nhất để không bị lỗi thời!

### rbenv
Mình dùng brew để cài, chạy những lệnh này trong terminal

```bash
brew install rbenv
```

```bash
rbenv init
```

Làm theo hướng dẫn sau khi gọi lệnh này, thêm dòng `eval "$(rbenv init -)"` vào cuối file `~/.bashrc`.

Đóng hết cửa sổ và mở một cửa sổ mới để ăn những thay đổi.

Kiểm tra xem thiết lập đã ổn chưa bằng lệnh này

```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash

Checking for `rbenv' in PATH: /usr/local/bin/rbenv
Checking for rbenv shims in PATH: OK
Checking `rbenv install' support: /usr/local/bin/rbenv-install (ruby-build 20170523)
Counting installed Ruby versions: none
  There aren't any Ruby versions installed under `~/.rbenv/versions'.
  You can install Ruby versions like so: rbenv install 2.2.4
Checking RubyGems settings: OK
Auditing installed plugins: OK
```

Ra giống giống như vầy là ok rồi đó! Tới lượt cài Ruby.

### Ruby
Bạn chạy lệnh này để cài ruby mới nhất ở thời điểm viết bài này.

```bash
rbenv install 2.6.3
```

Sau đó cài thêm bundler

```bash
gem install bundler
```

Bạn tạm thời set global cho phiên bản mới cài để dùng cho tất cả dự án

```bash
rbenv global 2.6.3
```

hoặc cài đặt cục bộ cho từng project riêng biệt, khi terminal đang ở thư mục cần thiết lập bạn chạy lệnh

```bash
rbenv local 2.6.3
```

Để kiểm tra xem Ruby đã được cài đúng chưa thì gõ lệnh

```bash
ruby -v

ruby 2.6.3p62 (2019-04-16 revision 67580) [x86_64-darwin18]
```

lấy ra phiên bản hiện tại của nó. (Chủ yếu là 2.6.3 còn mấy số loằn ngoằn sau thì tuỳ máy với phiên bản nữa nhá)

### Rails
Để cài Rails chạy lệnh

```bash
gem install rails -v 6.0.0.rc1
```

```bash
rails -v

Rails 6.0.0.rc1
```

ra như trên là ok rồi đó.

### Những thứ khác
Bạn cần cài thêm nodejs và yarn

Với nodejs bạn cứ việc lên trang chủ tải bộ cài về là ok. Mình đang dùng phiên bản 12.4.0, bạn có thể dùng phiên bản 10 cũng được.

Cũng như rbenv, mình dùng brew để cài yarn

```bash
brew install yarn
```

Còn một điểm nữa đó là cơ sở dữ liệu, mặc định thì Rails dùng sqlite đã được cài sẵn trong máy nên không cần cài thêm. Để test thì dùng sqlite ok, nhưng để làm cho sản phẩm thì thường người ta sẽ dùng MySql, Postgresql, MongoDb,... rất nhiều thứ ngoài kia đang chờ bạn khám phá nhưng tạm thời ở đây sqlite là đủ rồi.

## Khởi tạo dự án

Trong terminal, bạn đi đến thư mục muốn đặt project của mình, sau đó chạy lệnh 

```bash
rails new books-sharing

cd books-sharing
```

để tạo một thư mục chứa code, mình sẽ làm việc với thư mục này, books-sharing là tên project của mình.

Sau khi đã vào thư mục mới, bạn chạy các lệnh sau

```bash
bundle

rails webpacker:install
```

Chà, tới đây là xong phần cài đặt rồi, nếu mọi chuyện ổn thoả các bạn hãy bật server lên nào

```bash
rails s
```

Sau đó bạn mở trình duyệt vào đường dẫn `http://localhost:3000/`, nếu hiện lên màn hình thế này

![ss](https://raw.githubusercontent.com/driedjack/my-code-life/rails-tutorial/images/rails-tutorial-1.png)

có nghĩa là bạn đã chạy được một ứng dụng Rails trên máy mình rồi đó. Cùng ăn mừng nào. 🍳

## Tổng kết phần đầu

Thông qua bài giới thiệu và cài đặt này mình mong là các bạn nắm được những ý cơ bản về Rails framework viết bằng ngôn ngữ lập trình Ruby, để chạy được Rails cần thiết lập những gì cùng với cách giải thích và chia sẻ của series này. Ở phần tiếp theo chúng ta sẽ thử cho ứng dụng của mình xử lý vài yêu cầu đơn giản của người dùng xem sao nhé, à, và ta cũng sẽ dùng Bootstrap để giảm bớt css cho series này, chúng ta sẽ tập trung vào các khái niệm của Rails nhiều hơn, như vầy thì cũng phải xem them về gem nữa ấy, nói chung là nhiều thứ hay ho đang đợi các bạn lắm, keke. 🥳

Nếu có bất kì thắc mắc hay cần trợ giúp thì cứ thoải mái gửi mail cho mình nhé. Mail của mình luôn hiện diện ở dưới bên phải màn hình, thấy không?

Cảm ơn đã đọc bài nhé!
