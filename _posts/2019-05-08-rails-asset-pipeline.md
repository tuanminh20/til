---
template: post
title: Rails asset pipeline
author:
  name: DHS
---

## Asset pipeline là gì
Asset pipeline cung cấp việc concatenate và minify/compress JavaScript, CSS assets (Cũng có thể là CoffeeScript, Sass và ERB).
Mặc định nó đã có sẵn, có thể disable nếu không muốn dùng.

## Lợi ích
- Concatenate
  - combine thành  1 file => browser chỉ tốn 1 request để get assets
  - dùng fingerprint để tạo unique name cho mỗi file sau khi nối => cache hiệu quả hơn
- Minification or compression
  - xóa space, comment... để giảm size của file assets
- Precompilation
  - dịch bọn scss, coffee... thành css và js để dùng

**Bonus:**
Fingerprint là kỹ thuật sử dụng nội dung của file để tạo ra tên file unique, hỗ trợ cho caching.
Khi nội dung file thay đổi thì tên file cũng thay đổi.

Ví dụ: application.css sẽ được đổi tên thành 
```ruby
application-908e25f4bf641868d8683022a5b62f54.css
```
Đoạn hex kia đc gen ra bằng MD5 hexdigest từ nội dung của file.