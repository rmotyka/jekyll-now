---
layout: post
title: F# types in a nutshell
tags: F#
published: false
---

It is a large subject but I'll try to make a condensed pill.

| Type            | Sample                                    | Description         |
|-----------------|-------------------------------------------|---------------------|
| Alias           | type ProductId = int                      | Just another name.  |
| Tuple           | let item = (3, "dog")                     | No type keyword.    |
| Record          | type Person = {name: string; age:int}     | 