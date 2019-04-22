---
template: "post"
title: "Rails Request/Response Cycle"
date: "2019-04-22 18:51"
author:
  name: Minh
---

Request: Browser -> Nginx -> Puma/Unicorn -> Rack (Middleware) -> Routing -> Controller (-> Model) -> Views -> Controller (response được tạo ra)
Response: Controller -> Routing -> Rack (Middleware) -> Puma/Unicorn -> nginx -> Browser

## Tại sao cần cả Nginx và Puma/Unicorn

* Nginx gọi là web server. Puma/Unicorn gọi là Application server
* Tầng nginx thường sử lý để redirect
  * Nginx viết bằng C nên rất nhanh
  * Những request động cần vào server thì redirect request vào Puma/Unicorn
  * Những request tĩnh, ví dụ 404.html, 500.html, images tĩnh v.v, /public (phân biệt với /public ở Rails app) mà không cần khởi động application server (giả sử App server chết thì nginx vẫn trả về trang 500.html cho người dùng được)
  * http redirects to https
  * Xử lý multi threads, bundle nhiều request vào file gửi cho Puma/Unicorn hiệu quả về performance hơn
  * nginx xử lý DDos attack
* Tầng Puma/Unicorn
  * App server là thứ thật sự "chạy" app của bạn, chạy ở đây tức là load code và lưu nó ở trong memory.
  * Trong khi nginx không cần thiết ở môi trường development, application server bắt buộc ở cả môi trường dev/prod

## Rack là gì

### Tại sao cần Rack

Nói 1 cách đơn giản, Rack là bridge giữa App server và Rails app. Vì mỗi app server có thể nói 1 ngôn ngữ khác nhau. Nên cần 1 thằng ở giữa để dịch.

Ví dụ:

Puma gửi request object (Array) -> Rack -> Rails -> Rack nhận response object, parse thành Array -> Puma

Unicorn gửi request object (Hash) -> Rack -> Rails -> Rack nhận response object, parse thành Hash -> Unicorn

### Viết 1 Rack app đơn giản

```rb
class ComplimentApp
  def call(request)
    [200, {'Content-Type' => 'text/plain'}, ["I like your shoes"]]
  end
end
```

* Rack app bắt buộc phải có method `call`
* ở ví dụ này return 1 `Array` có 3 phần từ (phù hợp với format của Puma, mỗi App server có những quy định riêng về kiểu trả về, format của kiểu trả về).
  * Puma quy định phần tử thứ nhất là HTTP status code
  * Phần tử thứ 2 là 1 hash, giá trị của HTTP headers
  * Phần tử thứ 3 là 1 object `response_to` method `each`, chính là `body` của response

### Rack endpoint

Endpoint (đích đến) của Rack, tức là Rails Application được định nghĩa ở file `config.ru`

```rb
require_relative 'config/environment'
run Rails.application
```

`Rails.application` return instance của Rails App class, Rails App class lại được định nghĩa ở `application.rb`

```rb
module PLServer
  class Application < Rails::Application
  end
end
```

trường hợp này là return instance của `PLServer::Application` class. `PLServer::Application` kế thừa từ `Rails::Application`, một rack App. `Rails::Application` implement phương thức `#call` (nói ở trên)

```rb
def call(env)
  req = build_request env
  app.call req.env
end
```

> env ở đây không phải biến môi trường ENV, nó chính là request object

### Rack Middleware

Là một thành phần trong Rack. Có 2 nhiệm vụ:

* Sửa env object trước khi được pass vào endpoint tiếp theo
* Sửa response object trước được pass vào endpoint tiếp theo

Sau khi request đi qua khoảng 200 middleware objects, nó sẽ vào Router

### Routing

router dispatch request vào controller và action tương ứng
ở bước này thì đã có 1 empty response object được tạo ra rồi

```rb
controller_class.dispatch(action, request, response)
```

### Controllers

* chạy before_action
* logic trong action
* view rendering, stored in `response.body`,
* after_action

### Rời controllers

* chạy after_action
* `response.to_a` convert response object thành một array mà Rack hiểu được

### Rời routing

Routing dơn giản là sẽ pass response của controller vào Middleware, **trừ khi** `response.headers` include `X-Cascade: pass`. Khi đó Routing sẽ dispatch response object này sang một App khác chẳng hạn,
Vì dụ NodeJS app

### Rời Middleware

Middleware có thể sửa status code, headers, body ở phase này. Ví dụ:

* xóa toàn bộ response body nếu có `HTTP caching headers`
* set/edit cookie in response header

## Rails as backend, JS framework as frontend
