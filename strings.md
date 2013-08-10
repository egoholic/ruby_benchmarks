
```ruby
Benchmark.bm do |b|
  # <<
  b.report { string = "string"; 100_000.times { string << "e" } }
  b.report { string = "string"; 1_000_000.times { string << "e" } }
  b.report { 100_000.times { string = "s";  string << "e" } }
  b.report { 1_000_000.times { string = "s"; string << "e" } }
  # +=
  b.report { string = "string"; 100_000.times { string += "e" } }
  b.report { string = "string"; 1_000_000.times { string += "e" } }
  b.report { 100_000.times { string = "s"; string += "e" } }
  b.report { 1_000_000.times { string = "s"; string += "e" } }
end
```

```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   0.030000   0.000000   0.030000 (  0.033572)
   0.230000   0.000000   0.230000 (  0.233721)
   0.040000   0.000000   0.040000 (  0.038343)
   0.360000   0.000000   0.360000 (  0.366828)
   
   2.050000   0.000000   2.050000 (  2.056514)
 220.470000   6.050000 226.520000 (227.158200)
   0.050000   0.010000   0.060000 (  0.046953)
   0.470000   0.000000   0.470000 (  0.476070)
```

```ruby
Benchmark.bm do |b|
  b.report {}
  b.report
end
```
