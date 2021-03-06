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

### send vs explicid method call

```ruby
class B
  def a; end
end

Benchmark.bm do |x|
  x.report { obj = B.new; 1_000_000_000.times {obj.a} }
  x.report { obj = B.new; 1_000_000_000.times {obj.send :a} }
end
```

##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448

       user     system      total        real
  95.690000   0.000000  95.690000 ( 95.876045)
 111.440000   0.100000 111.540000 (111.837126)

explicit calls is 14.1% than send

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
  76.520000   0.000000  76.520000 ( 76.650600)
 102.940000   0.000000 102.940000 (103.093323)

explicit calls is 25.6% than send
```

### def vs define_method
```ruby
class A
  def a; end
  define_method(:b) {}
end

# For single method call
Benchmark.bm do |x|
  x.report { 100_000_000.times { A.new.b  } }
  x.report { 100_000_000.times {A.new.a} }
end
```
##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448

       user     system      total        real
  39.320000   0.000000  39.320000 ( 39.418881)
  28.160000   0.010000  28.170000 ( 28.230322)

def is 28.4% faster than define_method

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
  31.420000   0.000000  31.420000 ( 32.021655)
  25.480000   0.000000  25.480000 ( 25.547244)

def if 20.4% faster than define_method
```

```ruby
# For multiple method calls
Benchmark.bm do |x|
  x.report { obj = A.new; 100_000_000.times {obj.b} }
  x.report { obj = A.new; 100_000_000.times {obj.a} }
end
```
##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448

          user     system      total        real
  17.720000   0.050000  17.770000 ( 17.815660)
   9.280000   0.020000   9.300000 (  9.307244)

def is 47.6% faster than define_method

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
  12.770000   0.000000  12.770000 ( 12.823291)
   7.890000   0.000000   7.890000 (  7.910796)

def is 38.2 % faster than define_method
```

### #send vs Proc#call
```ruby
Benchmark.bm do |x|
  obj = 'string'
  lmbd = ->(obj) { obj.to_s }
  proc = Proc.new { |obj| obj.to_s }

  x.report { 1_000_000.times { obj.send(:to_s) } }
  x.report { 1_000_000.times { lmbd.call(obj) } }
  x.report { 1_000_000.times { proc.call(obj) } }
end
```
##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448

       user     system      total        real
   0.110000   0.000000   0.110000 (  0.108591)
   0.170000   0.000000   0.170000 (  0.171853)
   0.170000   0.000000   0.170000 (  0.177797)

#send is 35.7% faster, lambda and proc are equal

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   0.110000   0.000000   0.110000 (  0.109724)
   0.170000   0.000000   0.170000 (  0.169154)
   0.170000   0.000000   0.170000 (  0.173261)

#send is 35.7% faster, proc and lambda are equal
```

### method_missing with and without method definition
```ruby
class A
  def method_missing(method_name, *args, &block)
    if method_name.match /\Amethod_([\d])+\Z/
      $1
    else
      super method_name, *args, &block
    end
  end
end

class B
  def method_missing(method_name, *args, &block)
    if method_name.match /\Amethod_([\d])+\Z/
      B.class_eval <<-METHOD
        def #{method_name}
          $1
        end
      METHOD
    else
      super method_name, *args, &block
    end
  end
end

Benchmark.bm do |x|
  x.report {1_000_000.times { A.new.method_100500 }}
  x.report {1_000_000.times { B.new.method_100500 }}
end
```
##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448

       user     system      total        real
   1.930000   0.010000   1.940000 (  1.941696)
   0.360000   0.000000   0.360000 (  0.364693)

#method_missing with method definition is 81.2% faster.

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   2.090000   0.000000   2.090000 (  2.104916)
   0.330000   0.000000   0.330000 (  0.331562)

#method_missing with method definition is 84.3% faster.
It's faster because #method_missing have been called only once.
```

