### Method calls

```ruby
module A
  def a; :a end
end

module B
  include A
  def b; :b end
end

module C
  include B
  def c; :c end
end

module D
  include C
  def d; :d end
end

module E
  include D
  def e; :e end
end

class MyClass
  include E
  def f; :f end
end


Benchmark.bm do |b|
  b.report { 1_000_000.times { MyClass.new.a } }
  b.report { 1_000_000.times { MyClass.new.b } }
  b.report { 1_000_000.times { MyClass.new.c } }
  b.report { 1_000_000.times { MyClass.new.d } }
  b.report { 1_000_000.times { MyClass.new.e } }
  b.report { 1_000_000.times { MyClass.new.f } }
end
```

##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448

       user     system      total        real
   0.310000   0.000000   0.310000 (  0.312143)
   0.300000   0.000000   0.300000 (  0.302222)
   0.300000   0.000000   0.300000 (  0.304557)
   0.290000   0.000000   0.290000 (  0.304814)
   0.310000   0.000000   0.310000 (  0.306702)
   0.280000   0.000000   0.280000 (  0.301384)

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   0.250000   0.010000   0.260000 (  0.253295)
   0.230000   0.000000   0.230000 (  0.231801)
   0.240000   0.000000   0.240000 (  0.236016)
   0.240000   0.000000   0.240000 (  0.235877)
   0.240000   0.000000   0.240000 (  0.248153)
   0.250000   0.000000   0.250000 (  0.242689)
```


```ruby
class A
  def a; :a end
end

class B < A
  def b; :b end
end

class C < B
  def c; :c end
end

class D < C
  def d; :d end
end

class E < D
  def e; :e end
end

class MyClass < E
  def f; :f end
end


Benchmark.bm do |b|
  b.report { 1_000_000.times { MyClass.new.a } }
  b.report { 1_000_000.times { MyClass.new.b } }
  b.report { 1_000_000.times { MyClass.new.c } }
  b.report { 1_000_000.times { MyClass.new.d } }
  b.report { 1_000_000.times { MyClass.new.e } }
  b.report { 1_000_000.times { MyClass.new.f } }
end
```

##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448

       user     system      total        real
   0.290000   0.000000   0.290000 (  0.296654)
   0.290000   0.000000   0.290000 (  0.289507)
   0.290000   0.000000   0.290000 (  0.290083)
   0.280000   0.000000   0.280000 (  0.292860)
   0.290000   0.000000   0.290000 (  0.293749)
   0.290000   0.000000   0.290000 (  0.288914)

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   0.240000   0.000000   0.240000 (  0.246034)
   0.230000   0.000000   0.230000 (  0.231503)
   0.240000   0.000000   0.240000 (  0.232060)
   0.220000   0.000000   0.220000 (  0.229448)
   0.230000   0.000000   0.230000 (  0.230160)
   0.240000   0.000000   0.240000 (  0.233843)
```

