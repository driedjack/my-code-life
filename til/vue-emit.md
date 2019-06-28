App của mình có một component cha là một bảng render một component modal, modal này là component con. Modal có một nút khi ấn vào thì có khả năng đặt giá trị cho thuộc tính data của bảng. Tình huống này mình xử lý bằng `emit`.

Ví dụ

```html
// Modal Component
<template>
  <button class="btn" @click="$emit('play')">
    Chơi thôi
  </button>
</template>
```

```html
<template>
  <div>
    // Some custom code
    <caro-announce @play="blurIt">
    </caro-announce>
  </div>
</template>
<script>
export default {
  components: {},
  data: function () {
    return {
      isBlur: true
    }
  },
  methods: {
    blurIt: function() {
      this.isBlur = false
    }
  }
}
</script>
```

`emit` cho phép  tạo một event mới cho vue instance, khi gọi `emit` sẽ kích hoạt event đó. Ở đây trong component cha ta render modal và nói với nó rằng khi kích hoạt event `play` sẽ gọi hàm `blurIt`. Event `play` được khai báo trong model, khi click vào button, kích hoạt event `play`.

Thế thì nhờ có `emit` mà từ component con ta có thể điều chỉnh được trạng thái của component cha. Đó là một công dụng của `emit`.
