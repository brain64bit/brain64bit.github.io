---
layout: post
title: "Elixir Pattern Matching"
date: 2015-07-02 22:27:48 +0700
comments: true
categories: [elixir]
---
In Elixir pattern matching can be done using **=** operator, the equal sign compares left-hand side variable with right-hand side variable, it's success if both are equal. Lets see simple example:

```elixir
iex(1)> foo = 'bar'
'bar'
iex(2)> 'bar' = foo
'bar'
iex(3)> 'baz' = foo
** (MatchError) no match of right hand side value: 'bar'

iex(3)> foo = 'baz'
'baz'
iex(4)> 'baz' = foo
'baz'
```
<!--MORE-->
` 'bar' = foo ` is valid because both left & right side are equal to 'bar'. But in the line 3, when the sides don't match it's raise **MatchError**. From above example, variable can only be assigned on the left side.

```elixir
iex(10)> list = [1,[2,3],4]
[1, [2, 3], 4]
iex(11)> [x,y,z] = list
[1, [2, 3], 4]
iex(12)> y = [2,3]
[2, 3]
iex(13)> [2,4] = y
** (MatchError) no match of right hand side value: [2, 3]

iex(13)> x
1
iex(14)> z
4
iex(15)>
```
A pattern (the left side) is matched if the values (the right side) have the same structure and if each term in the pattern can be matched to the corresponding term in the values.

Also, you can ignoring a value when assignment with **underscore** operator '_'. It's useful when we didn't need to capture a value during assignment.

```elixir
iex(19)> [a, _] = [1, 2]
[1, 2]
iex(20)> a
1
iex(21)>
```
Another feature that exist in elixir pattern matching is **caret** ^ operator. It's useful if you want to force to Elixir to use existing value or you don't want to rebinding a variable. Just prefix a variable with ^ to use existing value.

```elixir
iex(30)> z = 5
5
iex(31)> [^z, 6] = [5, 6]
[5, 6]
iex(32)>
```
**Ref:**

* [Elixir Pattern Matching](http://elixir-lang.org/getting-started/pattern-matching.html)

