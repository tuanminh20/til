---
layout: post
title: Hash, Array, Set in Ruby
date: "2019/04/19 20:23"
---

|                       | Array                                                                                       | Hash                                                                      | Set                                  |
|-----------------------|---------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|--------------------------------------|
| index                 | dùng interger để indexing<br> ví dụ 0,1,2<br> -1 là last item<br> -2 là item kế item -1<br> |                                                                           |                                      |
| key                   | là index: 0,1,2                                                                             | key là cái đéo gì cũng được<br> từ int, string, symbol, object, class v.v | không có key                         |
| key unique?           | tất nhiên                                                                                   | tất nhiên                                                                 | không có key                         |
| nếu key không tồn tại | nil                                                                                         | nil                                                                       | không có key                         |
| value unique?         | không                                                                                       | không                                                                     | có, không tồn tại set mà chứa (1, 1) |
| lookup                | slow                                                                                        | fast                                                                      | fast                                 |
|                       |                                                                                             |                                                                           |                                      |