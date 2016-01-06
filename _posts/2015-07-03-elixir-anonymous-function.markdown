---
layout: post
title: "Elixir Anonymous Function"
date: 2015-07-03 01:08:21 +0700
comments: true
categories: [elixir] 
---
In Elixir we can create anonymous function like in javascript, use **fn** keyword to define an anonymous function.

```elixir
fn
  parameters -> body
end

fn (parameters) -> body end
```
Variable can be assigned by anonymous function, and you can call it using `variable_name.(arg1, arg2)` for example, below is a simple anonymous function that bind into a variable.

<!--MORE-->

```elixir
velocity = fn(distance, time) -> distance / time end
IO.puts "Speed of light is #{velocity.(600_000_000, 2)}"
```

## Function & Pattern Matching
Pattern matching can be applied in function, we can use pattern matching in function argument to determine which implementation to run. For example lets use tuple for argument.

```elixir
say = fn
  {:en, name} -> IO.puts "Hello #{name}"
  {:id, name} -> IO.puts "Halo #{name}"
end

say.({:id, 'Max'})
say.({:en, 'Rockatansky'})
```
Please try and guess what the output of the example above. The tuple structure in function argument determines which implementation will be executed. It's like one function with multiple bodies. Now lets implement fizz-buzz without conditional but using pattern matching:

```elixir
fizz_buzz = fn
  {0, 0, _} -> IO.puts "FizzBuzz"
  {0, _, _} -> IO.puts "Fizz"
  {_, 0, _} -> IO.puts "Buzz"
  {_, _, n} -> IO.puts n
end

is_fizz_buzz = fn
  (n) -> fizz_buzz.({rem(n,3), rem(n,5), n}) 
end

is_fizz_buzz.(10)
is_fizz_buzz.(11)
is_fizz_buzz.(12)
is_fizz_buzz.(13)
is_fizz_buzz.(14)
is_fizz_buzz.(15)
is_fizz_buzz.(16)
```
Can you guess what the output is?

