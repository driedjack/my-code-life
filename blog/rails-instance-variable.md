*Chào bạn blog, đã rất rất rất lâu rồi mới lại ngồi đây cùng bạn. Khoảng thời gian qua không bên bạn mình đã có một tí thay đổi, còn bạn giờ này thì vẫn như xưa chẳng đổi dời, và cảm xúc mà bạn cho mình vẫn vậy. Mấy thàng vừa qua mình ở trong tình trạng thất nghiệp, nhiều khi nghĩ tới bạn nhưng cũng vì mệt mỏi mà không ấn được nút enter để gặp bạn, vì những gì mình gửi gắm nơi bạn đều là nhiệt huyết, xúc cảm trong những ngày ngời ngợi nơi mình, mình không muốn phải nhận thêm sự bất lực nào nữa. Mong là lời chào không dài quá, bởi bạn thì chẳng nói bao giờ.*

Hiện tại mình mới nhận được một chân lập trình fresher trong một công ty nhỏ, mình nằm trong team *Ruby on Rails* viết web app. Công việc của mình hằng ngày là làm những cái task trong một dự án nhận từ khách hàng. Vì mình chỉ mới tiếp xúc với rails nên kiến thức và kỹ năng còn rất yếu và mình muốn cải thiện nó nên mình nghĩ ra một cách là sẽ viết ra những gì đã làm theo cách hiểu của mình về những task ấy lên đây vừa để mình có thể ôn lại kiến thức vừa để nhận góp ý từ các bạn cũng lập trình rails như mình, và biết đâu có ích cho những bạn khác cũng mới tìm đến rails. 😁

Post hôm này là một ghi chú nhỏ về *instance variable* trong controller

Trường hợp mình gặp là trình ra danh sách các notification trong webview của app. Khi người dùng đăng kí chụp ảnh thì thợ chụp sẽ nhận thông báo rồi khi thợ chụp đồng ý thì người dùng sẽ nhận, rồi thêm vài thông báo khác kiểu kiểu vậy. Trang hiển thị sẽ thêm một đốm sáng cho những thông báo nào chưa xem và các thông báo xem rồi thì không, và khi trình ra hết rồi thì sẽ update trạng thái của tất cả thông báo này là đã xem.

Trong controller của mình như này:

```ruby
class Webview::NotificationsController < Webview::ApplicationController
  def index
    @notifications = current_user.notifications.ordered_by_new.to_a
    unseen_notifications = current_user.notifications.with_status :sent
    unseen_notifications.update_all status: 'seen' if unseen_notifications.present?
  end
end
```

Biến `@notifications` sẽ được dùng trong view.

Đầu tiên mình gán tất cả notification của người dùng hiện tại được sắp xếp theo độ mới của nó, sau đó sẽ lọc ra những thông báo nào chưa được xem và nếu có thì sẽ update hết thành đã xem. (Mình dùng `enumerize` cho thuộc tính `status` với hai giá trị là `sent` và `seen`.

Lúc đầu code của mình không dùng hàm `to_a`, khi mình request đến page index thì lạ chưa, tất cả những status dù đọc hay chưa đọc đều không sáng. Mình thấy lạ hết mức vì trong controller mình đã tạo ra một biến dành riêng cho tất cả notification với trạng thái ban đầu rồi sau đó mình mới đặt một biến riêng cho những thằng chưa xem và chỉ update mỗi thằng đó thôi thì cái biến của mình phải còn như cũ, có những thằng chưa xem nữa chứ. Thế là mình chìm đắm trong men suy nghĩ nồng nàn =)) rồi vô tình đưa mắt tìm đến log

![log-1](https://tukhucxuan.files.wordpress.com/2018/05/screen-shot-2018-05-12-at-7-20-50-pm.png?w=1000)

Mình nhìn một lượt thấy cũng mượt, tới lượt thứ hai thì thấy có gì đó sai sai, các bạn có để ý là cái lệnh *load notification* mà có *ORDER BY* nó nằm phía dưới *Rendering* hông. Điểm mấu chốt là ở đó, khi mình gán giá trị cho thằng `@notifications` thì chỉ là gán điểm đến bộ nhớ cho nó thôi chứ lúc này chưa lấy giá trị từ database, kiểu giống tài khoản trong thẻ atm vậy, mình có 200 ngàn nhưng mà có ở trong thẻ chứ không có ngay trên tay, chỉ khi nào cần dùng mình mới đi rút thôi. Thế nên khi view dùng biến này nó sẽ gọi tới database thì lúc này đã update notification cả rồi.

Nên mình mới nghĩ có cách nào lấy tiền liền không hồi nào dùng thì dùng luôn khỏi phải đi rút. Mình thấy trong view chỉ gọi hàm `each` cho nó thôi và array thì cũng gọi được hàm này thế nên mình thêm `to_a` để lấy luôn giá trị hiện tại của notification, thế là xong. Đây là log với `to_a`

![log-2](https://tukhucxuan.files.wordpress.com/2018/05/screen-shot-2018-05-12-at-7-30-01-pm.png?w=1000)

Đó là những suy nghĩ của mình, bạn nào có cách làm hay hơn hay góp ý gì thì comment liền nha. 🤓

Lần này trở lại với bạn blog, chắc là mình sẽ có thêm vài chuỗi bài mới cho bạn đỡ mốc meo nha.

P/s: bonus design của chàng designer cực lúa 🤪

![screenshot](https://tukhucxuan.files.wordpress.com/2018/05/24eec88b-0b3f-4d53-b818-fcda486d733c.png?w=1000)
