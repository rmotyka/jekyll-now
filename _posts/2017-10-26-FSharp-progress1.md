---
layout: post
title: Current progress
tags: F#
published: true
---

Some new knowledge from F# world.
<!-- more -->

Book
=====

I've bought a [book](https://pragprog.com/book/swdddf/domain-modeling-made-functional) from great Scott Wlaschin. I's a beta publication, but I didn't want to wait for the final publication. The book covers some DDD theory and of course F# interpretation. 
There are some advanced stuff like functors etc. but of course in disguise. I hope my next business system I'll write in F#.


Euler project
=============

To make some F# kata I've started [Project Euler competition](https://projecteuler.net). Earlier I've tried a little [Programming Puzzles & Code Golf](https://codegolf.stackexchange.com/) but guys with J language took everything it was so demotivating.

Other stuff
===========

Unfold
-------

Generates sequence.
From MSDN samples:
1. 
Generates numbers from 0 to 20:

```F#
Seq.unfold (fun state -> if (state > 20) then None else Some(state, state + 1)) 0
```

2.
Genrates Fibonacci numbers:

```F#
let unfoldFib maxNumber (one, two) = 
    let nextNumber = one + two
    if (nextNumber > maxNumber) then None
    else Some(nextNumber, (two, nextNumber))
let fib maxNumber = Seq.unfold (unfoldFib maxNumber) (1,1)

fib 10000 |> Seq.toList
```
