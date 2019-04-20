---
layout: post
title: Instance variable in Rails
---

### Các loại biến

| Loại biến | ví dụ | độ phủ sóng | default |
|----------|----------|------|-----|
| global variable | `$name` | tất cả mọi nơi | `nil`
| class variable  | `@@name` | all object, class con | `undefined` |
| **instance variable** | `@name` | object self | `nil` |
| local variable | `name` | method | `undefined` |

### Định nghĩa instance variable
- Tên biến: bắt đầu bằng @, đi sau là tên ví dụ `@total_income`
- Phạm vi: bất kể đâu trong class, bất kì object nào được tạo từ class
- Intance variable không public, không dùng được ngoài object
- Giá trị mặc định nếu chưa được init là `nil`

### Nơi sử dụng
- Sử dụng trong method [Setter và Getter]({{ site.baseurl }}/2019/04/19/setter-and-getter-in-Ruby)
- Định nghĩa trong controller, dùng ở ngoài view bằng cơ chế <a href="https://books.google.com.vn/books?id=YGQ-DwAAQBAJ&pg=PT188&dq=how+rails+copy+instance+variable+to+view&hl=vi&sa=X&ved=0ahUKEwibwYWjuNzhAhWc8XMBHWlTBZcQ6AEIMDAB#v=onepage&q=how%20rails%20copy%20instance%20variable%20to%20view&f=false" target="_blank">controller-to-view data handoffs</a>
của Rails
