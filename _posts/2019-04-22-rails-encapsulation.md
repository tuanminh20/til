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

Bài này nói về Tính bao đóng: **Encapsulation**

## Trong OOP nói chung

* giữ tất cả thuộc tính, giá trị của object nằm trong object
* viết các phương thức setter, getter cho từng thuộc tính, giá trị chỉ khi có **nhu cầu sử dụng**

## Trong Rails nói riêng

Ứng dụng 1 trong Rails

* sử dụng `attr_reader`, `attr_writer`, and `attr_accessor` để access vào phương thức, giá trị của object
* khi chỉ cần read thì không nên sử dụng `attr_accessor` (có cả quyền read và write)

Ứng dụng 2 trong Rails: Strong params.

```rb
private

def dog_params
  params.require(:dog).permit(:name, :weight)
end
```

* Không cho phép các params khác được tham gia vào để init object dog, ví dụ `color`
* việc `private` cho method `dog_params` có thể coi như một phương thức bao đóng
