---
layout: post
title: "ruby inject method"
date: 2014-03-18 00:23:14 +0000
comments: true
categories: [ruby]
---
Ruby inject method is useful for combining an element inside enumerable, with binary operation and initial value.

For example inject method useful for accumulation in one dimensional array :

```ruby
[1,2,3].inject(0){ |initial_variable, element| element + initial_variable }
```
from above operation, 0 in inject argument is initial value of **initial_variable** if you forget to specify an argument in inject method by default first element in enumerable will be used as initial value.
<!-- MORE -->
From above example inject method will accumulate all element inside array / collection, which is :

```ruby
[1,2,3].inject(0){ |initial_variable, element| element + initial_variable} # => same as 1+2+3 = 6
```
Another example :

```ruby
[2,4,6,8].inject(10){ |initial, element| initial * element } # => 3840
```
Well, from above example you can simplify the operation process by giving operator symbol like **:+, :*, :/**

```ruby
[1,2,3].inject(:+) # => 6
[2,4,6].inject(:*) # => 48
```
Another awesomeness inject method is you can build hash key value from multi dimensional 2xN array, for example :

```ruby
[[:a,1],[:b,2],[:c,3]].inject({}){|init,elem| init.merge(elem.first => elem.last)  }
# => {:a=>1, :b=>2, :c=>3}
```
