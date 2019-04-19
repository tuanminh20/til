---
layout: "post"
title: "Modules in Ruby: include, extend, prepend"
date: "2019/04/19 18:42"
---

* include, prepend các hàm định nghĩa trong module trở thành instance method
* extend các hàm định nghĩa trong module trở thành class method

```ruby
module A
  def say
    puts "A"
  end
end

module B
  def say
    puts "B"
  end
end

module C
  def say
    puts "C"
  end
end

module D
  def say
    puts "D"
  end
end

module E
  def speak
    puts "speak in E"
  end

  def talk
    puts "talk in E"
  end
end


class Test
  include A # đưa module A vào ngay bên phải class Test trong danh sách ancestors
  include B # đưa module B vào ngay bên phải class Test trong danh sách ancestors
  prepend C # đưa module C vào đầu tiên trong danh sách ancestors 
  prepend D # đưa module D vào đầu tiên trong danh sách ancestors
  extend E # dduwa module E vào ngay bên phải singleton class Test

  def self.talk
    puts "talk in Test"
  end
end

Test.ancestors # [D, C, Test, B, A, Object, Kernel, BasicObject]

test = Test.new
test.say # D
# Lý do là tìm hàm theo thứ tự từ trái qua phải trong [D, C, Test, B, A, Object, Kernel, BasicObject]

test.speak
# undefined method 

Test.speak # speak in E
Test.talk # talk in Test

Test.singleton_class
# => #<Class:Test>
Test.singleton_class.ancestors
# => [#<Class:Test>, E, #<Class:Object>, #<Class:BasicObject>, Class, Module, Object, Kernel, BasicObject]
```