---
layout: post
title: Fold
tags: F#
published: false
---
That is a powerful one. Fold is a swiss army knife when you deal with lists etc. It is a kind of iterator over list items and during that process it gathers output in an accumulator value. So to use it you have to give necessary arguments:

* function or lambda which is executed in each iteration
* initial accumulator value and
* the list with items

This is a  example from msdn:
```F#
let data = [("Cats",4);
            ("Dogs",5);
            ("Mice",3);
            ("Elephants",2)]
let count = List.fold (fun acc (nm,x) -> acc+x) 0 data
printfn "Total number of animals: %d" count
```

1. List.fold is a function that we are talking about
1. (fun acc (nm,x) -> acc+x) is the lambda executed during iteration
1. 0 is the initial accumulator value
1. data is a list with items

The hardest part is the first argument: function or lambda. It takes two arguments accumulator and single item and it returns new accumulator value. The accumulator type does not need to be the same type as items form the list.

Ok, so lets use it.