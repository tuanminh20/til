---
template: "post"
title: "Ruby Strings vs Symbols"
date: "2019-04-22 10:15"
author:
  name: Minh
---

## Định nghĩa
```rb
"hello" # String
:hello # Symbol

"hello world!" # String with space and bang!
"hello world!".to_sym # String can convert to Symbol and vice versa
# => :"hello world!"
:"hello world!".class
# => Symbol
# Vậy là Symbol cũng có thể chưa kí tự đặc biệt
```

## Symbol can't change while **mutable** String can

```rb
a = "hello"
# => "hello"
b = :hello
# => :hello
a.object_id
# => 47213907316480
b.object_id
# => 1150428
a << "w"
# => "hellow"
a.object_id
# => 47213907316480
b << "w"
# NoMethodError: undefined method `<<' for :hello:Symbol
```

## Frozen Strings: **immutable** String
```rb
example = "hello world"
example.upcase!

puts example

example.freeze
example.downcase!

# => "HELLO WORLD"
# => *.rb:7:in `downcase!': can't modify frozen string (TypeError)
```

## Object ID

```rb
puts "hello world".object_id
puts "hello world".object_id
puts "hello world".object_id
puts "hello world".object_id
puts "hello world".object_id

# => 3102960
# => 3098410
# => 3093860
# => 3089330
# => 3084800

puts :"hello world".object_id
puts :"hello world".object_id
puts :"hello world".object_id
puts :"hello world".object_id
puts :"hello world".object_id

# => 239518
# => 239518
# => 239518
# => 239518
# => 239518
```

## Performance
* Khai báo Symbol nhanh hơn 40% so với khai báo String
* So sánh Symbol vs Symbol nhanh hơn 40% so với so sánh String vs String
