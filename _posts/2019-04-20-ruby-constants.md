---
template: "post" 
title: "Ruby Constants"
date: "2019-04-20 18:01:20"
author:
  name: Minh
---

## Định nghĩa

* Viết hoa chữ cái đầu
* Không thể định nghĩa trong method, chỉ bên trong Class, Module

```ruby
class RubyBlog
  URL    = "rubyguides.com"
  AUTHOR = "Jesus Castello"

  def the_method
    # ABC = 1 => Lỗi "dynamic constant assignment"
  end

  p RubyBlog::AUTHOR # "Jesus Castello"
end
```

## Tính bất biến

giá trị của constants có thể thay đổi, nhưng sẽ hiện cảnh báo
tốt nhất là nên `freeze` object trước khi assign
`freeze` khiến bạn không thể thay đổi giá trị của ô nhớ, hữu ích khi object là String, Array, Hash

```ruby
ABC = 1
ABC = 2
# 2: warning: already initialized constant ABC
ABC = 2.freeze
ABC = 1

puts ABC
# => 1

Minh = "Minh".freeze
Minh << "dep trai" # RuntimeError: can't modify frozen String
#
```

## Scope

```ruby
# Ví dụ về kế thừa
class A
  FOO = 1
end

class B < A
  def foo
    puts FOO
  end
end

B.constants # [:FOO]
B::FOO # 1

# Ví dụ 
module Food
  STORE_ADDRESS = "The moon"
  class Bacon
    def foo; puts STORE_ADDRESS; end
  end
end
fb = Food::Bacon.new
fb.foo # "The moon"

Food::STORE_ADDRESS
# => "The moon"

# STORE_ADDRESS không phải constant của Food::Bacon
Food::Bacon::STORE_ADDRESS
# NameError: uninitialized constant Food::Bacon::STORE_ADDRESS

module Mixin
  A = 123
end
class Product
  include Mixin
  puts A
end
# 123

class OtherProduct
  extend Mixin
  puts A
end

#  uninitialized constant OtherProduct::A

module Parent
  VALUE = "Parent"
  def print_value
    VALUE
  end
end

class Child
  include Parent
  VALUE = "Child"
end
# warning: already initialized constant Child::VALUE
# warning: previous definition of VALUE was here

Child::VALUE
# => "Child"
Child.new.print_value
# => "Parent"


class A
  FOO = 1
end
class A::B
  class C
    puts FOO
  end
end
# NameError: uninitialized constant A::B::C::FOO

class A
  class B
    class C
      puts FOO
    end
  end
end
# 1
```

