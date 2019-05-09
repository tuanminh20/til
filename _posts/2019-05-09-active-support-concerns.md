---
template: post
title: Rails ActiveSupport Concerns
author:
  name: Buffet
---

## Concern là gì
Concern thực chất là các đoạn code được tách nhỏ ra cho phép chúng ta có thể tổ chức code một cách mạch lạc, “sạch sẽ” hơn.

## Chức năng của Concern
### Chức năng của Concern = Module + extends methods in included callback

Module cho phép gom methods lại thành nhóm và sau đó các methods này được sử dụng bằng cách include module chứa chúng vào trong module/class khác.

```ruby
# ../model/concerns/engine.rb
module Engine
  def start_engine
    "Jarvis, start the engine!"
  end
end

# ../model/tank.rb
class Tank < ActiveRecord::Base
  include Engine

  def run
    "I'm running."
  end
end

# ../model/iron_man.rb
class IronMan < ActiveRecord::Base
  include Engine

  def snap_fingers
    "I am Iron Man!"
  end
end
```

Sử dụng
```ruby
$ rails c
$ puts Tank.new.start_engine
Jarvis, start the engine!
$ puts IronMan.new.start_engine
Jarvis, start the engine!
```

Hạn chế của việc class include module đó là class đó chỉ có thể truy cập tới instance methods của module mà không thể truy cập tới các class methods

Một cách giải quyết là ta nhóm các class methods trong một module khác và extend nó trong included callback, đồng thời trong callback này, có thể viết các method về validates, quan hệ, scope.. để các model có thể tái sử dụng

```ruby
# ../model/concerns/engine.rb
module Engine
  def self.included klass
    klass.extend ModuleMethods

    klass.class_eval do
      has_one :piston
      validates :engine_type, presence: true
    end
  end

  module ModuleMethods
    def missile_launch
      "Fire!!"
    end
  end

  def start_engine
    "Jarvis, start the engine!"
  end
end

# ../model/tank.rb
class Tank < ActiveRecord::Base
  include Engine
end

# ../model/iron_man.rb
class IronMan < ActiveRecord::Base
  include Engine
end
```

```ruby
$ rails c
$ puts IronMan.new.start_engine #OK
$ puts IronMan.missile_launch   #OK
```

Refactor lại 2 ví dụ trên sử dụng Concern

```ruby
# ../model/concerns/engine.rb
module Engine extend ActiveSupport::Concern
  included do
    has_one :piston
    validates :engine_type, presence: true

    def start_engine
      "Jarvis, start the engine!"
    end

    class << self
      def missile_launch
        "Fire!!"
      end
    end
  end
end

# ../model/tank.rb
class Tank < ActiveRecord::Base
  include Engine
end

# ../model/iron_man.rb
class IronMan < ActiveRecord::Base
  include Engine
end
```

### Giải quyết vấn đề về dependency
Trong ví dụ sau, module Bar sẽ phụ thuộc và module Foo, để sử dụng module Bar, cần include đồng thời cả Foo lẫn Bar trong class Test:

```ruby
# test_dependency.rb

module Foo
  def self.included base
    base.class_eval do
      def self.method_of_foo
        puts "inside method of Foo"
      end
    end
  end
end

module Bar
  def self.included base
    base.method_of_foo
  end
end

class Test
  include Foo # Cần include module này do module Bar phụ thuộc vào module Foo
  include Bar # Bar mới là module thực sự cần dùng
end
```

Khá là vô lý khi sử dụng một module mà còn phải quan tâm xem nó phục thuộc vào module nào khác nữa. Vì vậy có thể thử include trực tiếp module Foo trong module Bar

```ruby
# test_dependency_fail.rb

module Foo
  def self.included base
    base.class_eval do
      def self.method_of_foo
        puts "inside method of Foo"
      end
    end
  end
end

module Bar
  include Foo
  def self.included base
    base.method_of_foo
  end
end

class Test
  include Bar
end
```

==> gây lỗi `undefined method 'method_of_foo' for Test:Class`, bởi trong hoàn cảnh này, `base` là Bar chứ không phải class Test, nên chương trình sẽ không thực hiện đoạn code trong block base.class_eval

Giải quyết vấn đề này bằng việc dùng Concern

```ruby
# test_dependency_success.rb
require "active_support/concern"

module Foo extend ActiveSupport::Concern
  included do
    class << self
      def method_of_foo
        puts "inside method of Foo"
      end
    end
  end
end

module Bar extend ActiveSupport::Concern
  include Foo

  included do
    self.method_of_foo
  end
end

class Test
  include Bar # Class Test không quan tâm đến các module mà Bar phụ thuộc nữa
end
```
