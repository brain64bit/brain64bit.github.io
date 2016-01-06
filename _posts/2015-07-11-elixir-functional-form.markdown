---
layout: post
title: "Elixir Functional Form"
date: 2015-07-11 00:39:18 +0700
comments: true
categories: [elixir, functional]
---
I am still exploring Elixir anonymous function, let's move on to another interesting feature. Like javascript or functional programming language, in Elixir we can writes function that **takes one or more functions as an input** and or **returns a function**,

{% codeblock lang:elixir %}
iex(1)> passed_func = fn(a) -> a + 3 end
#Function<6.90072148/1 in :erl_eval.expr/5>
iex(2)> apply_func = fn(func_arg, num) -> func_arg.(func_arg.(num)) end
#Function<12.90072148/2 in :erl_eval.expr/5>
iex(3)> apply_func.(passed_func, 7)
13
iex(4)>
{% endcodeblock %}
We have **passed_func** which is a function that returns a function, and **apply_func** which is takes function as an argument. Inside apply\_func we apply passed\_func twice. 
<!--MORE-->
Like Ruby, inside Elixir Enum module has a function called map which has the same purpose, different with Ruby, in Elixir the map function takes two arguments, first is a collection and second is a function.

**ruby way:**

```ruby
irb(main):001:0> list = [1,2,3,4,5]
=> [1, 2, 3, 4, 5]
irb(main):002:0> list.map{|el| el * 2}
=> [2, 4, 6, 8, 10]
irb(main):003:0>
```

**elixir way**

```elixir
iex(5)> list = [1,2,3,4,5]
[1, 2, 3, 4, 5]
iex(6)> Enum.map list, fn(el) -> el * 2 end
[2, 4, 6, 8, 10]
iex(7)>
```

## The & Shortcut
Again like a Ruby, Elixir has & operator to write short function. The body of the function surrounded by ( and ), and the the placeholders &1, &2, and so on correspond to the first, second, and subsequent parameters of the function.

```elixir
iex(2)> add = &(&1 + &2)
&:erlang.+/2
iex(3)> add.(9, 4)
13
iex(4)>
```
This shortcut function can be applied in map function:

**ruby way**

```ruby
irb(main):009:0> list = %w<a b c d e>
=> ["a", "b", "c", "d", "e"]
irb(main):010:0> list.map(&:upcase)
=> ["A", "B", "C", "D", "E"]
irb(main):011:0>
```

**elixir way**

```elixir
iex(7)> list = ["a", "b", "c", "d", "e"]
["a", "b", "c", "d", "e"]
iex(8)> Enum.map list, &(String.upcase(&1))
["A", "B", "C", "D", "E"]
```
