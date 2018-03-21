---
layout: post
title: Giraffe i18n
tags: F#
published: true
---

My favourite web framework in F# is [Giraffe](https://github.com/giraffe-fsharp/Giraffe). The framework is well documented and easy to start with. Especially if you have background in C# ASP.NET Core. Adding internationalization is of cousre quite similar to C# version.
<!-- more -->
My solution has some assumptions. First I didn't want to have http context in every function that I use, but still I wanted to use i18 there. Second is that all ASP.NET Core tutorials show how to localize controllers - in Giraffe I don't use controllers and so I wanted some "global" or "shared" resoruce.


Packages
--------

Fist we need to add three Nuget packages:
- Microsoft.AspNetCore.Mvc.Localization
- Microsoft.AspNetCore.Localization
- Microsoft.Extensions.Localization

Standard procedure
------------------

Then according to standard procedure we need to add some code to configuration function:

```F#

let configureApp (app : IApplicationBuilder) =
    let supportedCultures = [| new CultureInfo("pl-PL"); new CultureInfo("en-GB") |]
    let localizationOptions = new RequestLocalizationOptions()
    localizationOptions.DefaultRequestCulture <- new RequestCulture(supportedCultures.[0])
    localizationOptions.SupportedCultures     <- supportedCultures
    localizationOptions.SupportedUICultures   <- supportedCultures
    app.UseRequestLocalization(localizationOptions) |> ignore
    
    GlobalStringLocalizerFactory <- app.ApplicationServices.GetService<IStringLocalizerFactory>()

```

The last line initialize global variable which is needed latter.

In services configuration we need to add:

```F#

let configureServices (services : IServiceCollection) =
    services.AddCors()    |> ignore
    services.AddGiraffe() |> ignore
    services.AddLocalization(fun options -> options.ResourcesPath <- "Resources") |> ignore

```

Custom code
-----------

In the top of file add global variable to keep reference to localizer factory, and a global localization function.

```F#

let mutable GlobalStringLocalizerFactory: IStringLocalizerFactory = null

let getLocalString key =
    let assemblyName = System.Reflection.Assembly.GetExecutingAssembly().GetName().Name
    let localizer = GlobalStringLocalizerFactory.Create("SharedResource", assemblyName)

    localizer.Item(key).Value

```

We also need to add some dummy class which name must be aligned to shared resource files.

```F#

type SharedResource () = 
    class 
    end

```

Resource files
--------------

Again standard procedure:

1. create folder Resource
1. in that folder create file SharedResource.pl-PL.resx and SharedResource.en-GB.resx.

Sure, that have to be aligned with our supported cultures.

Usage
-----

Anywhere in your code you can call *getLocalString* with the key which can be found in resources files.
If the key can't be found there the key will be returned.

Tip: you can use default phrase as a key.

The code can be found [here](https://github.com/rmotyka/Giraffei18n)

At the end I also would like to thank [damienbod](https://damienbod.com/2017/11/01/shared-localization-in-asp-net-core-mvc/) who inspired me for that solution.
