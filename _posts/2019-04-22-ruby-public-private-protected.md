---
template: "post"
title: "Ruby Public, Private, Protected methods"
date: "2019-04-22 10:56"
author:
  name: Minh
---

## Public

Public method có thể được gọi bên trong hay bên ngoài class

```rb
class Animal
  def intro_animal
    "I am a #{self.class}"
  end
end

Animal.new.intro_animal
# => "I am a Animal"
```

## Private

* Private method chỉ có thể được gọi bên trong class
* Khi có sự kế thừa, behaviour không có gì thay đổi

```rb
class Animal
  def intro_animal
    class_name
  end
  private
  def class_name
    "I am a #{self.class}"
  end
end

n = Animal.new
n.intro_animal #=>I am a Animal
n.class_name #=>error: private method `class_name' called
```

## Protected vs Private

* Protected method chỉ có thể được gọi bên trong class, giống Private
* không gọi được qua self.private_method, nhưng có thể gọi self.protected_method
* Có sự khác biệt khi có sự kế thừa

```rb
class Animal
  def animal_call
    protect_me
  end
  protected
  def protect_me
    p "protect_me called from #{self.class}"
  end  
end

# giống Private
n= Animal.new
n.animal_call #=> protect_me called from Animal
n.protect_me #=>error: protected method `protect_me' called

class Mammal < Animal
  def mammal_call
    protect_me
  end
end

# giống Private
n= Mammal.new
n.mammal_call #=> protect_me called from Mammal


class Amphibian < Animal
  def amphi_call
    Mammal.new.protect_me #Receiver same as self
    self.protect_me  #Receiver is self
  end   
end

# Khác Private, có thể gọi thông qua self, lưu ý: Mammal.new same as self
n= Amphibian.new
n.amphi_call #=> protect_me called from Mammal
             #=> protect_me called from Amphibian  


class Tree
 def tree_call
   Mammal.new.protect_me #Receiver is not same as self
 end
end

# Vì Tree không kế thừa nên Mammal.new khác self
n= Tree.new
n.tree_call #=>error: protected method `protect_me' called for #<Mammal:0x13410c0>
```

## Send methods
`private`, `protected` methods có thể gọi thông qua method `send`
```rb
class Animal
  def intro_animal
    class_name
  end
  private
  def class_name
    "I am a #{self.class}"
  end
end

n = Animal.new
n.intro_animal #=>I am a Animal
n.class_name #=>error: private method `class_name' called
n.send("class_name") #=> I am a Animal
n.public_send("class_name") #error: private method `class_name' called
```

## Trong Rails
* Ở controller, chỉ các action (index, new, create v.v) nên là public methods, tất cả nên là private
* Ở model, `protected` methods hữu ích khi class B, class C đều kế thừa từ class A. Ở class C bạn muốn gọi method `protected` với receiver là class B
