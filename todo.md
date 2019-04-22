1. Module và Class giống và khác? - Minh
2. Array và Hash và Set giống và khác? - Minh
3. Symbol và String giống và khác? - Minh
4. Method setter và getter trong ruby - Son
5. 4 tính chất OOP
6. Private, protected và public - Minh
7. Class method và object method (cả ruby và rails)
8. Luồng xử lí của 1 request từ browser đi qua những tầng nào
9. Instance variable là gì?
10. 7 verb method HTTP
11. Thin controller & fat model
12. Cách tổ chức source code của 1 project Rails
13. Helper, decorator, service, lib
14. Assets precompile
15. Worker, sidekiq, cronjob
16. Concern
17. Associations
18. ORM
19. N+1 query và cách xử lí
20. Cache
21. AWS basic
22. Deploy

class Polygon
  @@sides = 10
  def self.sides
    @@sides
  end
end
class Triangle < Polygon
end

puts Triangle.sides # => 3
puts Polygon.sides # => 3
