---
layout: post
title: Setter & Getter in Ruby
---

### Setter và Getter là gì
- Setter là method dùng để gán giá trị cho instance variable
- Getter là method dùng để lấy giá trị của instance variable

### Setter và Getter tự định nghĩa
```ruby
class Cat
  def initialize(name)
    @name = name
  end
end

cat = Cat.new("Mimi")
cat.name #=> undefined method `name'
cat.name = "Mymy"
cat.name #=> undefined method `name='
```

```ruby
class Cat
  def initialize(name)
    @name = name
  end

  def name #getter method
    @name
  end

  def name=(name) #setter method
    @name = name
  end
end

cat = Cat.new("Mimi")
cat.name #=> "Mimi"
cat.name = "Mymy"
cat.name #=> "Mymy"
```

### Setter và Getter sử dụng accessors

```ruby
class Cat
  attr_reader: :name # creates the getter methods
  attr_writer: :name # creates the setter methods
  attr_accessor: :name # creates both setter & getter methods

  def initialize(name)
    @name = name
  end
end

cat = Cat.new("Mimi")
cat.name #=> "Mimi"
cat.name = "Mymy"
cat.name #=> "Mymy"
```

|                     | attr_reader            | attr_writer                   |attr_accessor                              |
|---------------------|------------------------------------|-------------------------------------|-------------------------------------|
| methods được tạo ra | `name`                             | `name=`                               |`name` <br> `name=`                               |
