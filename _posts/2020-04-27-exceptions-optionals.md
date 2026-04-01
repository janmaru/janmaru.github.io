---
title: "Exceptions Are Optionals"
date: 2020-04-27
author: Mauro Ghiani
category: Odds
tags: [FunctionalProgramming, CSharp, FSharp, Exceptions, Options, OOP]
excerpt: "It seems to be wrong to throw an exception for business logic, even if it's the standard view in OOP. The Functional approach with Options or Maybe can help a lot."
---

Talking about social distancing and the two-meter rule we don't deal with velocity speed close to the light speed. Time Dilation and Length Shrinkage are not involved and don't help here.

The same problem that comes to light happens with coding. Context arises when talking about this line of code or that.

One of the problems that always bothered me is that of "**Exceptions**" in code.

For instance, the term Guard or Guard Class is quite simple, it's a class with a collection of rules that throw exceptions as soon as some conditions are met.

"Try" has no *performance side-effects* unless an exception is thrown, or even not! Some people have pointed out that to some extent no optimizations are performed in try blocks on particular frameworks. But even so, it's the burden it has to be accepted when working with Java o C#. If there was any worry about **memory or extreme optimization,** it should have been used C or C++.

The point is... it seems to be **wrong** to throw an exception for business logic, even if it's the standard view in OOP. Also, what happens when someone decides to suppress the error before it is logged or displayed somehow to the client?

I feel that the Functional approach with **Options** or **Maybe** can help a lot in screening the code, for instance against null "boom" factors.

In F#, an option looks like this and it's impossible to result a null object, since also **None** is a value.

Exceptions should be limited to the boundaries of the code, for instance, IO or networking, but should not be present in the **business logic** or validation scopes.

I ported this philosophy into the C# world easily, now you can write a validation method with the function Bind or Options as in the F# world.

The repo is here:

[https://github.com/janmaru/mahamudra-core](https://github.com/janmaru/mahamudra-core)
