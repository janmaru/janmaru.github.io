---
title: "Eine kleine Netmusik"
date: 2020-05-09
author: Mauro Ghiani
category: Odds
tags: [CategoryTheory, DotNet, CSharp, FunctionalProgramming]
excerpt: "Today we are going to study the category .NET Core, the algebraic structure of types and functions we normally use in our dev daily life."
---

*When renowned philosopher U. G. Krishnamurti came to visit Ramana Maharshi he knew that uncountable people had experienced a tremendous peace in his presence.*

*But he felt nothing. So after a while, he asked: "Swami, are there any steps to reach the stage of liberation?"*

*And Ramana answered: "There are not such steps nor stages. If it's there, it is absolute and unchangeable. On the other hand, if it's not there, everything is missing."*

*U. G. Krishnamurti seemed unsatisfied: "Sir, I'm asking can you give me what you have?"*

*Ramana Maharshi answered: "I can give it!" Then almost casually: But, can you take it?*

*So the real point about Category Theory it's not whether it appeals to you or not, but whether you can take it or not.*

Today we are going to study the category **.NET Core,** the algebraic structure of types and functions we normally use in our dev daily life:

- obj(DotNetCore): the collection of objects in DotNetCore are **.NET Core** types, like string (System.String), bool (System.Boolean), int (System.Int32), etc.
- hom(DotNetCore): the collection of morphisms/relations/arrows/maps in DotNetCore category are **.NET Core** functions/methods between the input type (source object) to the output type (target object)
- A **composition** operation of morphisms denoted **[]**: in DotNetCore category, the composition operation of morphisms is the composition of functions.

Tho there are many ways to picture a formal description of the algebraic structure, we should enforce it once imagined the compositionality, associativity, and rule of identification.

For instance, we could use Reflection to pull out all **.net core** *types* and declare our objects!

We could define a simple morphism on the base that we should have an input type and an output type.

And then build the **delegate** associate with it.

So the identity morphism is done.

We imagine how a blueprint or result of a composition should be.

And then build the delegate associate with it.

At the end we could unit test if morphisms can compose.

Also, for identities.

Finally, we could check the associativity law.

The relevance of Category Theory shines everywhere, from LINQ to pure *functional programming.*

For the code in the article you will find everything here:

[janmaru/mahamudra-core](https://github.com/janmaru/mahamudra-core)
