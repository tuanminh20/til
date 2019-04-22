---
template: "post"
title: "Inheritance in Rails"
date: "2019-04-22 16:00"
author:
  name: Minh
---

Trong OOP có 4 khái niệm là:
* **A**bstraction (trừu tượng)
* **P**olymorphism (đa hình)
* **I**nheritance (kế thừa)
* **E**ncapsulation (Bao đóng)

Cách nhớ là: 4 kí tự đầu tiên tạo thành chữ: `A PIE`

Bài này nói về Tính kế thừa: **Inheritance**

## Trong OOP nói chung

* Class Con sẽ kế thừa những **thuộc tính**, **phương thức**, **constants** của class Cha, class Ông
* có thể override
* `super` trong method Con sẽ gọi method cùng tên trong class Cha, không tìm thấy thì sẽ tìm đến Ông, v.v

## Trong Rails nói riêng

Model kế thừa từ ApplicationRecord
ApplicationRecord kế thừa từ ActiveRecord::Base
