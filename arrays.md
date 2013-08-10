### '#concat' VS '+='
```ruby
Benchmark.bm do |b|
  # #concat
  b.report { array = [1, 2, 3]; 10_000.times { array.concat [4] } }
  b.report { array = [1, 2, 3]; 100_000.times { array.concat [4] } }
  b.report { 10_000.times { array = [1, 2, 3]; array.concat [4] } }
  b.report { 100_000.times { array = [1, 2, 3]; array.concat [4] } }

  # +=
  b.report { array = [1, 2, 3]; 10_000.times { array += [4] } }
  b.report { array = [1, 2, 3]; 100_000.times { array += [4] } }
  b.report { 10_000.times { array = [1, 2, 3]; array += [4] } }
  b.report { 100_000.times { array = [1, 2, 3]; array += [4] } }
end
```

##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   0.000000   0.000000   0.000000 (  0.001897)
   0.030000   0.000000   0.030000 (  0.027551)
   0.000000   0.000000   0.000000 (  0.005046)
   0.060000   0.010000   0.070000 (  0.054393)

   0.170000   0.000000   0.170000 (  0.177127)
  20.060000   0.380000  20.440000 ( 20.495631)
   0.000000   0.000000   0.000000 (  0.002617)
   0.050000   0.000000   0.050000 (  0.051487)
```

