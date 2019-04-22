---
template: "post"
title: "Polymorphism in Rails"
date: "2019-04-22 13:02"
author:
  name: Minh
---

Trong OOP có 4 khái niệm là:
* **A**bstraction (trừu tượng)
* **P**olymorphism (đa hình)
* **I**nheritance (kế thừa)
* **E**ncapsulation (Bao đóng)

Cách nhớ là: 4 kí tự đầu tiên tạo thành chữ: `A PIE`

Bài này nói về Tính đa hình: **Polymorphism**

## Trong OOP nói chung

Đa hình nghĩa là khi mà 1 phương thức có những **hành vi khác nhau**

## Trong Rails nói riêng

Các giải thích dễ hiểu như sau:

Có một class là `Manager`, một class là nhân viên `Employee`

`Manager` thì đi **ô tô**. Nhân viên thì đi **xe máy**. Về bản chất thì đây đều là xe/phương tiện đi lại. Sẽ có những **phương thức chung** như là: `start`, `drive`, `stop` v.v. Và tất nhiên cách `start` của xe máy và oto sẽ khác nhau rồi.

Nên về thiết kế thì mình sẽ làm như sau

```rb
# table managers
class Manager
  has_many :xes, as: :chu_phuong_tien
end

# table employees
class Employee
  has_many :xes, as: :chu_phuong_tien
end

# table xes
class Xe
  belongs_to :chu_phuong_tien, polymorphic: :true

  def start
    # ở đây ta có thể định nghĩa tuỳ theo đây là oto hay xe máy =))
  end
  def drive; end
  def stop; end
end
```

khi đó bảng `xes` thì ta sẽ có các cột:

```rb
  t.string  :name
  t.integer :chu_phuong_tien_id
  t.string  :chu_phuong_tien_type
```

ta cũng có những phương thức tương ứng:

```rb
  m = Manager.new
  e = Employee.new

  m.xes
  e.xes

  xe = Xe.new(name: "Teslaaaaa", chu_phuong_tien_id: 1, chu_phuong_tien_type: "Manager")
  xe.chu_phuong_tien # => Manager with id=1
```
