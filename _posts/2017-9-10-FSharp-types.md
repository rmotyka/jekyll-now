---
layout: post
title: F# types in a nutshell
tags: F#
published: true
---

It is a large subject but I'll try to make a condensed pill.
<!-- more -->

| Type                | Sample                                        | Remarks                |
|---------------------|-----------------------------------------------|------------------------|
| Alias               | type ProductId = int                          | Just another name.     |
| Tuple               | let person = (int, string)                    | No type keyword.       |
| Record              | type Person = {name: string; age:int}         | Can be nested.         |
| Discriminated Union | type Vehicle = Car of string \| Boat of string | Sth. like union in C++ |
| Enum                | type State = Good = 1 \| Bad = 2               | Like in C#             |
| Class               | type Order (number: int) =                    |                        |
|                     |      member ths.Print =                       |                        |
|                     |        printfn "Order %d" number              |                        |
| Interface           | type ICommand =                               |                        |
|                     |    abstract member this.Execute : unit -> unit|                        |
| Struct              | type Point =                                  |                        |
|                     |     struct                                    |                        |
|                     |       val x: int                              |                        |
|                     |       val y: int                              |                        |
|                     |     end                                       |                        |

It seems that the first four types are usually used in F# to model data. The next are rather to interop with C#.