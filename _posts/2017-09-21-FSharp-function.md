---
layout: post
title: One matching trick
tags: F#
published: true
---

Reading some pro F# code I encountered some unclear case. So I've made some investigation and here we are.
For the beginning regular matching:

```F#
let multByOption (a: int) (b: int option) =
    match b with
    | Some x -> Some (a * x)
    | None -> None
```

Simple function that multiply integer by the option integer.
But it might be written shorter:

```F#
let multByOption1 (a: int) =
    function
    | Some x -> Some (a * x)
    | None -> None
```

And the second function does the same.

And this is a form of that function between those above:

```F#
let multByOptionA (a: int) =
    fun (b: int option) ->
        match b with
        | Some x -> Some (a * x)
        | None -> None
```

So it is a first case with curring. Personally I'll stay with the first version as it is most clear what's going on.
