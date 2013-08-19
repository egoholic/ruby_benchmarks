# #to_s VS #to_sym

```ruby
Benchmark.bm do |b|
# #to_s
b.report { s = :symbol;  100_000_000.times { s.to_s } }
b.report { s = 'string'; 100_000_000.times { s.to_s } }
# #to_sym
b.report { s = :symbol;  100_000_000.times { s.to_sym } }
b.report { s = 'string'; 100_000_000.times { s.to_sym } }
end
```

##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
  18.970000   0.000000  18.970000 ( 19.010987)
   7.190000   0.000000   7.190000 (  7.201850)

   6.620000   0.000000   6.620000 (  6.633185)
  15.330000   0.000000  15.330000 ( 15.353015)
```

### '<<' VS '#concat' VS '+='

"#<<" и "#concat" - один и тот же метод

```c
    /* string.c */
    rb_define_method(rb_cString, "concat", rb_str_concat, 1);
    rb_define_method(rb_cString, "<<", rb_str_concat, 1);
```

```ruby
Benchmark.bm do |b|
  # <<
  b.report { string = "string"; 100_000.times { string << "e" } }
  b.report { string = "string"; 1_000_000.times { string << "e" } }
  b.report { 100_000.times { string = "s";  string << "e" } }
  b.report { 1_000_000.times { string = "s"; string << "e" } }
  # #concat
  b.report { string = "string"; 100_000.times { string.concat "e" } }
  b.report { string = "string"; 1_000_000.times { string.concat "e" } }
  b.report { 100_000.times { string = "s";  string.concat "e" } }
  b.report { 1_000_000.times { string = "s"; string.concat "e" } }
  # +=
  b.report { string = "string"; 100_000.times { string += "e" } }
  b.report { string = "string"; 1_000_000.times { string += "e" } }
  b.report { 100_000.times { string = "s"; string += "e" } }
  b.report { 1_000_000.times { string = "s"; string += "e" } }
end
```

##### Results
```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   0.030000   0.000000   0.030000 (  0.034561)
   0.240000   0.000000   0.240000 (  0.235563)
   0.030000   0.000000   0.030000 (  0.038720)
   0.370000   0.000000   0.370000 (  0.367721)

   0.030000   0.000000   0.030000 (  0.025119)
   0.240000   0.000000   0.240000 (  0.248420)
   0.040000   0.000000   0.040000 (  0.038987)
   0.380000   0.000000   0.380000 (  0.374734)

   1.880000   0.000000   1.880000 (  1.884921)
 194.760000   3.720000 198.480000 (198.951135)
   0.050000   0.000000   0.050000 (  0.047692)
   0.440000   0.000000   0.440000 (  0.442454)
```

### '<<' VS interpolation
```ruby
Benchmark.bm do |b|
  # <<
  b.report { 100_000.times { "string" << "string" } }
  b.report { 1_000_000.times { "string" << "string" } }

  # interpolation
  b.report { 100_000.times { "string #{"string"}" } }
  b.report { 1_000_000.times { "string #{"string"}" } }
end
```

##### Results

```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   0.050000   0.000000   0.050000 (  0.046931)
   0.360000   0.000000   0.360000 (  0.365020)

   0.010000   0.000000   0.010000 (  0.017804)
   0.170000   0.000000   0.170000 (  0.165179)
```

### '+' VS '+=' VS interpolation
```ruby
Benchmark.bm do |b|
  # +
  b.report { 100_000.times { s = "string"; s = s + "string" } }
  b.report { 1_000_000.times { s = "string"; s = s + "string" } }
  b.report { 10_000_000.times { s = "string"; s = s + "string" } }

  # +=
  b.report { 100_000.times { s = "string"; s += "string" } }
  b.report { 1_000_000.times { s = "string"; s += "string" } }
  b.report { 10_000_000.times { s = "string"; s += "string" } }

  # interpolation
  b.report { 100_000.times { s = "string"; s = "#{s} string" } }
  b.report { 1_000_000.times { s = "string"; s = "#{s} string" } }
  b.report { 10_000_000.times { s = "string"; s = "#{s} string" } }
end
```

##### Results

```
# Ubuntu 13.04 64-bit
# Intel® Core™ i5-2450M CPU @ 2.50GHz × 4
# RAM 7,7 GiB
# Ruby 2.0.0-p247

       user     system      total        real
   0.050000   0.000000   0.050000 (  0.050247)
   0.440000   0.000000   0.440000 (  0.447004)
   4.520000   0.000000   4.520000 (  4.529093)

   0.040000   0.000000   0.040000 (  0.047300)
   0.450000   0.000000   0.450000 (  0.445441)
   4.430000   0.000000   4.430000 (  4.448898)

   0.040000   0.000000   0.040000 (  0.037083)
   0.350000   0.000000   0.350000 (  0.348501)
   3.440000   0.000000   3.440000 (  3.449518)
```

