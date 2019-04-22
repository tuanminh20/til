---
template: "post"
title: "Abstraction in Rails"
date: "2019-04-22 15:43"
author:
  name: Minh
---

Trong OOP có 4 khái niệm là:
* **A**bstraction (trừu tượng)
* **P**olymorphism (đa hình)
* **I**nheritance (kế thừa)
* **E**ncapsulation (Bao đóng)

Cách nhớ là: 4 kí tự đầu tiên tạo thành chữ: `A PIE`

Bài này nói về Tính trừu tượng: **Abstraction**

## Trong OOP nói chung

Trừu tượng hóa điểm tương đồng của các đối tượng để tạo Class là Base
Ví dụ, class Ô tô, Xe máy, Xe đạp kế thừa từ Class Phương Tiện
class Chó, Mèo kế thừa từ class Động Vật

## Trong Rails nói riêng

```rb
class SomeAbstractModel < ApplicationRecord
  self.abstract_class = true

  # some heritable methods here
end
```

AbstractModel không có table tương ứng, nên không tạo object được

```rb
SomeAbstractClass.create!
# => NotImplementedError: (Vehicle is an abstract class and cannot be instantiated)
```

Cách dễ hình dung nhất về Abstract Class trong Rails là `ActiveRecord::Base`, nó không có bảng,
nó được sử dụng để các Model kế thừa nó, nên các Model có những phương thức nó định nghĩa,
như `find`, `where`

```rb
class AbstractModel < ActiveRecord::Base  
  self.abstract_class = true
end

class Foo < AbstractModel
end

class Bar < AbstractModel
end

# File activerecord/lib/active_record/base.rb, line 610
def find(*args)
  options = args.extract_options!
  validate_find_options(options)
  set_readonly_option!(options)

  case args.first
    when :first then find_initial(options)
    when :last  then find_last(options)
    when :all   then find_every(options)
    else             find_from_ids(args, options)
  end
end
```

## Abstract Class và Module

Xét bài toán sau, có class `Car`, class `Airplain` v.v, có chung attribute `weight`, method `convert_weight`
Nhưng cũng có attribute khác nhau, `Car` thì có bánh, `Airplain` thì không có bánh, mà có cánh quạt
Thiết kế như nào?

* Vấn đề của STI: sẽ có cột empty

```rb
class Vehicle < ApplicationRecord
  with_options presence: true, allow_blank: false do
    validates :weight
    validates :color
  end

  def convert_weight(unit)
    case unit
    when :lbs
      weight * 2.20462
    when :g
      weight * 1000.0
    end
  end
end

class Car < Vehicle
  validates :number_of_wheels, presence: true, allow_blank: false
end

class Airplane < Vehicle
  validates :number_of_wings, presence: true, allow_blank: false
end
```

migration cho bảng `vehicles`

```rb
create_table :vehicles do |t|
  # columns required by some children
  t.integer :number_of_wheels
  t.integer :number_of_wings
  # columns required by all children
  with_options null: false do
    t.string  :type # required for inheritence
    t.integer :weight
    t.string  :color
  end
end
```

Như vậy thì thằng `Car` thì `number_of_wings` null, thằng `Airplane` thì `number_of_wheels` null. Khi càng có nhiều attribute khác biệt thì càng có nhiều cột null

* Abstract Class xử lý vấn đề đó như thế nào

```rb
class Vehicle < ApplicationRecord
  self.abstract_class = true
  with_options presence: true, allow_blank: false do
    validates :weight
    validates :color
  end
  def convert_weight(unit)
    case unit
    when :lbs
      weight * 2.20462
    when :g
      weight * 1000.0
    end
  end
end
```

Khác ví dụ trên thì dữ liệu của `Car`, `Airplain` sẽ lưu ở bảng `cars`, `airplains` tương ứng. Không có bảng `vehicles`

```rb
class CreateVehicles < ActiveRecord::Migration[5.2]
  def change
    create_table :cars do |t|
      with_options null: false do
        t.integer :weight
        t.string  :color
        t.integer :number_of_wheels
      end
    end
    create_table :airplanes do |t|
      with_options null: false do
        t.integer :weight
        t.string  :color
        t.integer :number_of_wings
      end
    end
  end
end
```

> **Lưu ý 1**: Ở abstract model thì đôi khi chỉ khai báo prototype của method, logic cụ thể được định nghĩa ở Class con

> **Lưu ý 2**: Các phương thức ở Abstract class có thể được đóng gói vào Module
