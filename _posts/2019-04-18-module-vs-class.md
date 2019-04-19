---
layout: post
title: Module vs Class in Ruby
---

|                     | class                                  | module                                                               |
|---------------------|----------------------------------------|----------------------------------------------------------------------|
| có thể tạo object?  | có                                     | không                                                                |
| dùng để??           | để tạo ra object<br>để class khác kế thừa | gộp các method lại, để tái sử dụng cho class làm namespace cho class |
| superclass là gì?   | Object                                 | không có                                                             |
| có hàm gì?          | class method instance method           | module method  instance method                                       |
| kế thừa được không? | có                                     | không                                                                |
| có thể include?     | không                                  | có, include module trong class                                       |
| có thể extend?      | không                                  | có, extend module trong class                                        |

* class include module thì object sẽ có các methods được định nghĩa trong module, **instance method**
* class extend module thì methods sẽ đc extend như một **class method**

cụ thể hơn thì xem thêm ở bài [Modules in Ruby: include, extend, prepend]({{ site.baseurl }}/2019/04/19/modules-in-ruby-include-extend-prepend/)


```ruby
module A
  def a_method
    puts "a_method"
  end
end

module B
  def b_method
    puts "b_method"
  end
end

class Human
  include A
  extend B
end

Human.new.a_method
# a_method

Human.new.b_method
# NoMethodError: undefined method `b_method' for #<Human:0x00559a416bc4f8>

Human.a_method
# NoMethodError: undefined method `a_method' for Human:Class

Human.b_method
# b_method
```
