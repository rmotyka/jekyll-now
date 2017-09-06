---
layout: post
title: More folds
tags: F#
published: false
---
There are brothers and sisters of *List.fold*.

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

