---
layout: post
title: F# basics
tags: F#
published: false
---

## Code organization

### Namespaces

Like C# namespaces e.g.:

```F#
namespace Multidata.ProjectX.Backend
```

It must be a first declaration in the file. Namespace can contain only:

* types
* nodules

### Modules

*module* it is something like static class in C#. If there is one module in the class we can join namespace and module:

```F#
module Multidata.ProjectX.Backend.DataRepositoryModule
```


