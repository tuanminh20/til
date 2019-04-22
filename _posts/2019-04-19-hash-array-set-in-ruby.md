---
layout: post
title: Hash, Array, Set in Ruby
date: "2019/04/19 20:23"
author:
  name: Minh
---

|                           | Array                                                                                           | Hash                                                                                        | Set                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| **index**                 | dùng **interger** để indexing<br> ví dụ 0,1,2<br> -1 là last item<br> -2 là item kế item -1<br> | dùng **object_id** của key để indexing                                                      |                                                                         |
| **key**                   | là **index**: -2,-1,0,1,2                                                                       | key là cái đéo **gì cũng được**<br> từ int, string, symbol, object, class v.v               | **không có** key                                                        |
| **access element**        | array[index]<br> array[0]                                                                       | hash[key] hash["key_name"] hash[:key_name]                                                  | **không thể** access được 1 object cụ thể trong set<br> vì không có key |
| **key unique?**           | tất nhiên                                                                                       | tất nhiên                                                                                   | không có key                                                            |
| **nếu key không tồn tại** | nil                                                                                             | nil<br> có thể custom **default value**<br> ví dụ:<br> a = Hash.new(0) a[:some_key] # => 0  | không có key                                                            |
| **value unique?**         | không                                                                                           | không                                                                                       | **có**, không tồn tại set mà chứa (1, 1)                                |
| **lookup**                | slow                                                                                            | fast                                                                                        | **fastest**                                                             |
|                           |                                                                                                 |                                                                                             |                                                                         |
