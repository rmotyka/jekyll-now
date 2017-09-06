---
layout: post
title: Fold
tags: F#
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

```F#
let numbers = [1..10]
```

It's just a list with ten numbers from one to ten.

Fold a'la map
-------------

That is something like the "map" function. It iterates all items and multiply each one by the factor.

```F#
let mult1 factor numbers =
    List.fold (fun acc x -> acc @ [x * factor]) [] numbers
```

The accumulator is initialized by the empty list *[]* and for each item it appends *@* to the accumulator item multiplied by the factor.

Fold a'la filter
----------------

Filter in JavaScript or *where* in C# linq.

```F#
let where predicate numbers =
    List.fold (fun acc x -> if predicate x then acc @ [x] else acc) [] numbers
```

Again the accumulator is initialized with empty list, but in the lambda function the predicate is evaluated with each item. In F# *if .. then .. else* is an expression so the *else* is obligatory. It returns accumulator with appended item it predicate is true and accumulator without "modifications" if false.

The dark side of implementations above is that the list is immutable and so in each iteration it is copied again and again.

In the [msdn page](https://msdn.microsoft.com/pl-pl/library/ee353894(v=vs.120).aspx) on List.fold there are more useful samples e.g.:

```F#
let reverseList list = List.fold (fun acc elem -> elem::acc) [] list
printfn "%A" (reverseList [1 .. 10])
```

In lambda function instead of appending the operator *::* is pre-pending items to the accumulator.
Nice.
But fold can be use to aggregate the list somehow. 

Fold a'la sum
-------------

```F#
let sumList list = List.fold (fun acc elem -> acc + elem) 0 list
printfn "Sum of the elements of list %A is %d." [ 1 .. 3 ] (sumList [ 1 .. 3 ])
```

Again that is a sample from [msdn page](https://msdn.microsoft.com/pl-pl/library/ee353894(v=vs.120).aspx).
In each iteration it just add item to the accumulator, which initial value is zero.

If I find some interesting fold matters I'll add them here, but for now it seems to be enough to work with *List.fold*.