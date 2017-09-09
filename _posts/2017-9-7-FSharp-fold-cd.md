---
layout: post
title: More folds
tags: F#
published: true
---
There are brothers and sisters of *List.fold*.
<!-- more -->
List.foldBack
----------

For example *List.foldBack* which works the same as regular fold but it iterates the list from its end to start, but warning: the arguments are in different order than in List.fold, why??

```F#
List.fold      (fun acc x -> printf "%d " x ) () [1..10]
List.foldBack (fun x acc -> printf "%d " x ) [1..10] ()
```

Results:

```F#
1 2 3 4 5 6 7 8 9 10 val it : unit = ()
10 9 8 7 6 5 4 3 2 1 val it : unit = ()
```

1. But in fold *folder* function there are: *acc, x* and in foldBack: *x, acc*
1. The second difference order of init accumulator value and the list of items

List.fold2
--------

And also List.foldBack2 and there are some more functions with *2*. They do the same but on two lists at the same time.
Example:

```F#
List.fold2 (fun acc x y -> printf "%d%O " x y) () [1..3] ['a'..'c']
1a 2b 3c val it : unit = ()
```

So the folder function takes three arguments: accumulator, item from the first list and item from the second one. The length of these list must be the same.
In List.foldBack2 arguments are in different order, like in foldBack:

```F#
List.foldBack2 (fun y x acc -> printf "%d%O " x y) ['a'..'c'] [1..3] ()
3c 2b 1a val it : unit = ()
```

It seems that the logic is the same but all arguments are the opposite way. Good.