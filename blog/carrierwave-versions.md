Hổm này lận đận chuyện phòng trọ nên không viết bài được, nay mới chuyển sang phòng mới với mấy nhóc em nên viết một bài cho khuây khoả. Post này là về việc thao tác process ảnh với các version khác nhau dùng gem carrierwave.
Mô tả task như này, về việc đăng tải ảnh sẽ tự động xử lý một vài thao tác trên hình gốc như resize, optimize, thêm watermark,.. và trên version thumbail cũng vậy có điều là trên thumbnail không có watermark. Và cũng như thường lệ cách mình làm trong bài là mình google mà ra nên chắc chắn là không tối ưu với linh hoạt mấy nên rất cần sự góp ý của các bạn. 🤓

Code ban đầu:

```ruby
class PhotoUploader < ApplicationUploader
  process :watermark
  process :dynamic_optimize
  process resize_to_fit: [800, 800]
  process :store_dimensions

  version :thumb do
    process resize_to_fit: [400, 400]
  end

  def watermark
  # ...
  end
  
  def dynamic_optimize
  # ...
  end
  
  def store_dimensions
  # ...
  end
end
```

Khi upload ảnh gốc sẽ được gắn watermark, optimize, rezise, lưu kích thước, và sẽ rezise nhỏ hơn cho ảnh thumbnail. Vấn đề ở đây là carrierwave process ảnh trước khi gọi tới version.

> One important thing to remember is that process is called before versions are created. This can cut down on processing cost.
> <p>&mdash; Carrierwave</p>

Thế nên thumbnail cũng sẽ được optimize và gắn watermark luôn. Đó là khó khăn thật sự, nếu version và bản gốc độc lập với nhau thì dễ rồi, mình sẽ cho xử lí riêng hết, nên mình mới nghĩ là chia hai version luôn, một cho original một cho thumb, như này

```ruby
class PhotoUploader < ApplicationUploader
  version :original do
    process :watermark
    process :dynamic_optimize
    process resize_to_fit: [800, 800]
    process :store_dimensions
  end

  version :thumb do
    process :dinamic_optimize
    process resize_to_fit: [400, 400]
    process :store_dimensions
  end

  def watermark
  # ...
  end
  
  def dynamic_optimize
  # ...
  end
  
  def store_dimensions
  # ...
  end
end
```

Làm kiểu này dễ hiểu và nhanh nữa, nhưng có điều là lúc này đã có tới hai version với một ảnh gốc, khi cần gọi tới version có watermark phải thêm đuôi original nhưng như thế thì phải sửa lại code của bên khách hàng nên không dùng được cách này.

Lúc này mình mới nghĩ tới việc sẽ để nguyên ảnh gốc không xử lý mà sẽ xử lý thumbnail trước sau đó mới quay lại đóng watermark cho nó sau. Vì việc này không theo thông lệ nên phải dùng callback. Carrierwave cung cấp cho mình năm loại callback, mình cũng không nắm chắc những callback này thực hiện khi nào nữa. Nó đây: `cache`, `retrieve_from_cache`, `store`, `retrieve_from_store`, `remove`. Trong trường hợp này mình dùng call back `after :cache`, callback này xảy ra trước khi ảnh được thêm vào database, một callback khác có thể dùng được là `before :store`, nhưng lại là xử lí sau khi lưu, mình thấy kì kì nên thôi chọn `after :cache`.

```ruby
class PhotoUploader < ApplicationUploader
  version :thumb do
    process :dynamic_optimize
    process resize_to_fit: [400, 400]
    process :store_dimensions
  end

  after :cache, :watermark_original

  def watermark_original(_arg)
    return unless thumb
    watermark
    dynamic_optimize
    resize_to_fit 800, 800
    store_dimensions
  end

  def watermark
  # ...
  end
  
  def dynamic_optimize
  # ...
  end
  
  def store_dimensions
  # ...
  end
end
```

Ban đầu khi upload ảnh gốc sẽ không được xử lí, mà sẽ đi vào xử lí version thumb luôn, xong xuôi sẽ lại gọi ảnh gốc ra rồi đóng watermark vào. Hàm `watermark_original` sẽ return luôn nếu hình không có version thumb, nghĩa là khi xử lí lần đầu thì thumb vẫn chưa có nên sẽ không đóng watermark cho cả hai phiên bản, lần sau khi gọi ra thì lúc này đã có version thumb nên code sẽ chạy tiếp trong hàm và chỉ đóng cho mỗi thằng gốc thôi.
Mình làm như này thì ra được kết quả mong muốn, nhưng có vài chi tiết không hay ở đây là nhìn vào đống code rối ghê, rồi xử lí như này sẽ lâu hơn và tốn tài nguyên máy hơn khi phải moi hình cũ ra lại mà trang trí cho nó. Mình chỉ nghĩ được tới đây rồi phải nộp task vì tới hạn, ~~bạn nào có cách hay hơn thì comment giúp mình nha, để mình sửa cho bên họ luôn~~.

Cám ơn đã đọc bài của mình. 🤪
