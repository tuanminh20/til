---
template: post
title: Ruby's Ancestor Chain
author:
  name: Buffet
---

Để xem "gia phả" của một class => gọi hàm `ancestors` của class đó. Mảng trả về chứa thông tin theo thứ tự sau:
* calling class
* Module mà class đó include
* Ông già của class đó
* Module mà ông già của class đó include
* Ông già của ông già của class đó (ông nội :v)

Ví dụ:

```ruby
irb> String.ancestors
 => [String, Comparable, Object, Kernel, BasicObject]
```