## Tại sao dùng thuộc tính key cho v-for

Khi vue cập nhật trạng thái của DOM nó sẽ không cố gắng sắp xếp danh sách theo thứ tự mà sẽ thay thế tại chỗ luôn, có nghĩa là các phần tử DOM vẫn ở nguyên vị trí đó và vue sẽ cố gắng thay đối dữ liệu sao cho khớp với mong muốn tại các vị trí index. Mục đích của vue làm như vậy là để tăng performance nhưng sẽ không phù hợp với trường hợp mà đầu ra phụ thuộc component con. Có một vài ví dụ thường gặp như render bảng với danh sách data khi dữ liệu được cập nhật, như thêm hàng hay xoá hàng, đổi data các thứ thì sẽ có trường hợp dữ liệu được render không đúng.

Với những trường hợp này cần phải thêm thuộc tính `key` với giá trị duy nhất để vue có thể theo dõi từng node riêng biệt sau đó có thể dùng lại hoặc sắp xếp lại mà không lẫn lộn với node khác. Đa phần các trường hợp đều được khuyên dùng `key` với `v-for`. Nên nhớ chỉ dùng giá trị số và chữ cho `key`.

Ngoài ra còn nhiều trường hợp khác dùng `key`, đơn cử một trường hợp mình có dùng cho bàn cờ caro. Mình dùng bảng với các ô `td`, có một nút để kích hoạt chơi lại game, mình không muốn phải reload lại page để clear data cũ cũng như không muốn đi qua hết các ô `td` để xoá dữ liệu. Mình chỉ cần cho bảng đó một thuộc tính `key`, khi ấn vào nút chơi lại sẽ truyền vào key một giá trị khác, lúc này bảng được render lại hoàn toàn.

## Không dùng chung v-for với v-if trên cùng một node

Vì sao? Khi dùng chung với nhau thì `v-for` sẽ được ưu tiên trước `v-if` nếu các bạn không biết sẽ dẫn đến nhiều trường hợp không mong muốn, có thể render sai, hoặc lỗi, hoặc giảm performance. Ví dụ một trường hợp, bạn có danh sách những việc cần làm và bạn chỉ muốn list ra những công việc nào chưa làm xong. Có thể bạn sẽ viết

```html
<todo v-for="todo in todos" v-if="!todo.completed">
```

Vue sẽ chạy qua tất cả các phần tử trong `todos` rồi cứ mỗi lần render sẽ check liệu đã làm xong hay chưa, như vậy sẽ rất tốn tài nguyên và thời gian. Thay vì thế bạn có thể định nghĩa một `computed` gồm những việc chưa làm xong rồi cho lặp qua danh sách đó mà không cần dùng `if`.

Vẫn còn nhiều thứ hay ho về render danh sách trong vue, các bạn có thể tham khảo thêm ở [vue guide](https://vuejs.org/v2/guide/list.html) , [vue api](https://vuejs.org/v2/style-guide/#Avoid-v-if-with-v-for-essential), ... 
