---
layout: post
title: Property based testing
tags: F#
published: true
---

That subject concern not only functional programming languages, but it is very popular in that environament.
In F# comunity the most popular is [FsCheck](https://fscheck.github.io/FsCheck/)
<!-- more -->
Althought the project is good documented, from my perspective it is not quite clear how to use it.
Probalby it is a much larger topic, but as I'm accustomed to [XUnit](https://xunit.github.io/) I've found two main ways.

Property
--------

Example form FsCheck page:

```F#
open FsCheck.Xunit

[<Property>]
let ``Reverse of reverse of a list is the original list ``(xs:list<int>) =
  List.rev(List.rev xs) = xs

```

And the library will generate *xs* data for you. If you need some more control over it, you have to make some type with static function generating data. 

Again the sample from the FsCheck site (Running test section):

```F#
type Positive =
    static member Double() =
        Arb.Default.Float()
        |> Arb.mapFilter abs (fun t -> t >= 0.0)

type Negative =
    static member Double() =
        Arb.Default.Float()
        |> Arb.mapFilter (abs >> ((-) 0.0)) (fun t -> t <= 0.0)

[<Properties( Arbitrary=[| typeof<Negative> |] )>]
module ModuleWithProperties =

    [<Property>]
    let ``should use Arb instances from enclosing module``(underTest:float) =
        underTest <= 0.0

    [<Property( Arbitrary=[| typeof<Positive> |] )>]
    let ``should use Arb instance on method``(underTest:float) =
        underTest >= 0.0
```        

Traditional Fact
----------------

Another way to use FsCheck with XUnit are regular tests:

```F#
[<Fact>]
let ``Quantified Properties``() =
    let ints = Arb.from<int * int> |> Arb.filter(fun (x, y) -> x > 0 && y > 0)
    let adderProp = Prop.forAll ints (fun (x, y) ->  (printfn "x %d y %d" x y); x + y = x + y)
    Check.Quick adderProp
```

The second way is for me a little more clear, as all the test elements are in one function.
As you can see there are three elements: data generator, property preparation and property running.

Here are some my remarks:
-------------------------

- Always use *Check.QuickThrowOnFailure* or *Check.VerboseThrowOnFailure* as it makes your test red on failures
    The second function additionally print test data, which I've found very usefull.
- To see console output form XUnit I use CLI: *dotnet xunit -verbose*. It works but you have to have some items in .fsproj:

```XML
  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.6.0" />
    <PackageReference Include="xunit" Version="2.3.1" />
    <PackageReference Include="xunit.runner.visualstudio" Version="2.3.1" />
    <PackageReference Include="FsUnit.xUnit" Version="3.1.0" />
    <DotNetCliToolReference Include="dotnet-xunit" Version="2.3.1" />
  </ItemGroup>
```

- You might also want to create global.json file in main folder with sdk version:

```JSON
{
  "sdk": {
    "version": "2.0.2"
  }
}

```

- Some generators that were usable for me:
    - let l = Gen.choose(2, 12) |> Gen.nonEmptyListOf |> Gen.resize 5 |> Arb.fromGen 
        (that generates non empty list of size 5 with numbers from 2 to 12)
    -     let g = gen {
           let! digits = Gen.choose (0, 20) |> Gen.listOf |> Gen.resize 5 
           let! inputBase = Gen.choose (2, 20)
           let! outputBase = Gen.choose (2, 20) 

           return (digits, inputBase, outputBase)
        }
        (that generates tuple with three elemetns)

    -  let! files = Arb.Default.String () |> Arb.filter (fun s -> s <> null) |> Arb.toGen  
        (if you want to have some strings, but without nulls)

Bogus
-----

BTW there is a great test data generating library [Bogus](https://github.com/bchavez/Bogus) and you can combine some data form there:

```F#
    let systemGenerator = new Bogus.DataSets.System()
    let g = gen{
        let! files = Gen.choose (1, 30) |> Gen.map (fun i -> systemGenerator.DirectoryPath() + "/" + systemGenerator.FileName ()) |> Gen.listOf |> Gen.resize 5 
        let! testRun = Arb.generate<bool>

        return (files, testRun)
    }

```

That generates list of files, and some bool flag.

Happy coding. Thank you.
