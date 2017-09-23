---
layout: post
title: About lists functions part 1
tags: F#
published: true
---

It seems that List Module has plenty of interesting stuff. I suppose that it might be a very often used module I'll try to cover most of it.<!-- more -->
The source documentation is on [msd page](https://msdn.microsoft.com/visualfsharpdocs/conceptual/collections.list-module-%5bfsharp%5d).

append
------ 

Joins two list together. You can do that with List.concat as well. Another option is to use *@* operator.

```F#
let list1 = List.append [1; 2] [3; 4]
let list2 = List.concat [ [5; 6 ]; [7; 8] ]
let list3 = list1 @ list2
```

Very similar is *concat* which concatenate more than two lists:

```F#
[[1; 2]; [3; 4]; [5; 6]] |> List.concat
```

average
------

Argument list should not to be int, so if we have ints we should convert them earlier.

```F#
List.map (float) [0; 1; 1; 2] |> List.average
```

and also we have averageBy:

averageBy
-------

Before calculating average the given function is called.

```F#
List.averageBy (fun elem -> float elem) [1 .. 10]
```

choose
-----

That one is F# specific. It has a function which convert list items to option and then return only list of Some values.

```F#
List.choose (fun x -> if x > 2 then Some(x) else None) [ 2; 1;  3; -2; 4]
val it : int list = [3; 4]
```

another usage could be the case if you have a list with options:

```F#
List.choose (fun x -> x) [ Some(2); None; Some(3); None; Some(4)]
val it : int list = [2; 3; 4]
```

chunkBySize
------------

That's nice! Sometimes you have a long list and you want to divide it to smaller chunks. Here you are:

```F#
List.chunkBySize 3 [1..10]
val it : int list list = [[1; 2; 3]; [4; 5; 6]; [7; 8; 9]; [10]]
```

The first parameter is of course the size of the chunk.

That's enough for now, but I have to return to List module later it has tons of nice features.

collect
-------

For each element in list collect apply a function and then concatenate the result. It might be used on two kinds:

* To concatenate list of lists:

```F#
[[1; 2; 3]; [4; 5; 6]; [7; 8; 9]; [10]] |> List.collect (id)
```

* To generate list for each list element and concatenate the result:

```F#
[1; 2; 3] |> List.collect (fun x -> [1 .. x])
val it : int list = [1; 1; 2; 1; 2; 3]
```

contains
--------

Returns true if element is in given list.

```F#
List.contains "a" ["a"; "b"; "c"]
```

countBy
--------

Counts lambda functions results.

```F#
List.ofSeq "aaaabccaadeeee" |> List.countBy (id)
val it : (char * int) list =
  [('a', 6); ('b', 1); ('c', 2); ('d', 1); ('e', 4)]

// or

List.ofSeq "aaaabccaadeeee" |> List.countBy (fun x -> if x = 'a' then "A" else "X")
val it : (string * int) list = [("A", 6); ("X", 8)]
```

slicing
--------

In the meantime I've found something nice: _slicing_.

```F#
let l = [1..5] // val l : int list = [1; 2; 3; 4; 5]
//                             index: 0; 1; 2; 3; 4
l.[1..3] // val it : int list = [2; 3; 4]
l.[2..]  // val it : int list = [3; 4; 5]
l.[..3]  // val it : int list = [1; 2; 3; 4]
l.[..5]  // IndexOutOfRangeException
l.[-1..] // IndexOutOfRangeException
```

We can cut out some part of the list using index, as shown above. It's a new feature int F# 4.0.
If you use index outside of range you will get _System.IndexOutOfRangeException_ which is sound.