Như trong trang hướng dẫn của vuejs có nói trong một vài môi trường như Google Chrome Apps, có áp dụng _Content Security Policy (CSP)_ cấm việc dùng `new Function()` để tính toán biểu thức, thế nên, tính năng này của vuejs sẽ không dùng được. 

```javascript
document.addEventListener('DOMContentLoaded', () => {
  const app = new Vue({
    el: '#hello',
    data: {
      message: "Can you say hello?"
    }
  })
})
```

Bạn có để ý là khi khởi tạo dự án rails với webpacker và vuejs thì trong file mẫu có viết sẵn đoạn code như này

```javascript
import Vue from 'vue'
import App from '../app.vue'

document.addEventListener('DOMContentLoaded', () => {
  const app = new Vue({
    render: h => h(App)
  }).$mount()
  document.body.appendChild(app.$el)

  console.log(app)
})
```

Webpacker dùng hàm `render` để render app. Nhưng nếu dùng cách này thì sẽ không trỏ được tới element trong template có sẵn mà cần _single file components_ như đoạn code đầu tiên. Để giải quyết bài toán đó ta dùng **webpack + vue-loader** để precomlie thành hàm `render`

Nên sau này nếu deploy lên server nhưng không thấy code vuejs chạy và không thấy log lỗi gì trên console thì có thể bạn gặp phải trường hợp này đó!

Nguồn tham khảo thêm:
- [Vuejs guide](https://vuejs.org/v2/guide/installation.html#CSP-environments)
- [Webpacker](https://github.com/rails/webpacker#vue)
- CSP trên [Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
