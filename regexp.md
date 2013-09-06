### Precompiled regexp vs non-precompiled
```ruby
Benchmark.bm do |x|
  x.report { regexp = /my_string[0-9]+/; 10_000_000.times { |t| "my_string_#{t}" =~ regexp } }
  x.report { 10_000_000.times { |t| "my_string_#{t}" =~ /my_string[0-9]+/ } }
end
```
##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448
       user     system      total        real
   7.990000   0.000000   7.990000 (  8.011321)
   7.640000   0.010000   7.650000 (  7.673300)

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   7.850000   0.000000   7.850000 (  7.872886)
   7.400000   0.000000   7.400000 (  7.423450)

So Ruby does not precompile regexp like JS does.
```

### =~ vs #match
```ruby
Benchmark.bm do |x|
  x.report { 10_000_000.times { |t| "my_string_#{t}" =~ /my_string[0-9]+/ } }
  x.report { 10_000_000.times { |t| "my_string_#{t}".match /my_string[0-9]+/ } }
end
```
##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448

       user     system      total        real
   7.680000   0.000000   7.680000 (  7.710648)
   9.120000   0.000000   9.120000 (  9.146549)

=~ is 15.8% faster than #match

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   7.390000   0.000000   7.390000 (  7.416702)
   9.520000   0.000000   9.520000 (  9.554610)

=~ 22.4% faster than #match
```


### \A and \Z vs ^ and $
```ruby
Benchmark.bm do |x|
  x.report { 10_000_000.times { |t| "my_string_#{t}" =~ /my_string[\d]+/ } }
  x.report { 10_000_000.times { |t| "my_string_#{t}" =~ /\Amy_string[\d]+\Z/ } }
  x.report { 10_000_000.times { |t| "my_string_#{t}" =~ /^my_string[\d]+$/ } }
end
```
##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 1.9.3-p448

       user     system      total        real
   7.430000   0.000000   7.430000 (  7.445127)
   7.410000   0.000000   7.410000 (  7.438655)
   7.460000   0.000000   7.460000 (  7.481719)

# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   7.560000   0.000000   7.560000 (  7.593032)
   7.650000   0.000000   7.650000 (  7.675099)
   7.530000   0.040000   7.570000 (  7.575124)
```

