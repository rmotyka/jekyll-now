---
layout: post
title: About lists
tags: F#
published: false
---

It seems that List Module has plenty of interesting stuff. I suppose that it might be a very often used module I'll try to cover most of it.
The source documentation is on [msd page](https://msdn.microsoft.com/visualfsharpdocs/conceptual/collections.list-module-%5bfsharp%5d).

append
-----

Joins two list together. You can do that with List.concat as well. Another option is to use *@* operator.

```F#
let list1 = List.append [1; 2] [3; 4]
let list2 = List.concat [ [5; 6 ]; [7; 8] ]
let list3 = list1 @ list2
```