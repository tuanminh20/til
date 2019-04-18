---
layout: post
title: Module vs Class in Ruby
---

|                     | class                              | module                              |
|---------------------|------------------------------------|-------------------------------------|
| thể hiện của nó là? | object                             | không                               |
| superclass là?      | module                             | object                              |
| methods             | class methods and instance methods | module methods and instance methods |
| kế thừa             | có                                 | không                               |
| include trong class | không thể include                  | có thể include module trong class   |
| extend              | không thể                          | có thể                              |

* class include module thì object sẽ có các methods được định nghĩa trong module, **instance method**
* class extend module thì methods sẽ đc extend như một **class method**


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
