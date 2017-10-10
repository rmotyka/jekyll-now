---
layout: post
title: All combinations
tags: F#
published: true
---

In previous post I used a function from fssnip.net: [F# Snippets](http://www.fssnip.net/2z/title/All-combinations-of-list-elements).
I think it might be simpler.
<!-- more -->
It is that:

```F#
let allCombinations lst =
    let rec comb accLst elemLst =
        match elemLst with
        | h::t ->
            let next = [h]::List.map (fun el -> h::el) accLst @ accLst
            comb next t
        | _ -> accLst
    comb [] lst
```

but the recursion is just a loop so I think the simpler version mitht be with the List.fold:

```F#
let allCombinations lst =
    let calcComb accLst h =
        [h]::List.map (fun el -> h::el) accLst @ accLst
    List.fold calcComb [] lst
```

or even with such one-liner:

```F#
let allCombinations3 lst =
    List.fold (fun accLst h -> [h]::List.map (fun el -> h::el) accLst @ accLst) [] lst
```

IMHO the second version is most clear. Not always less code means better code, but recursion should be used when necessary.