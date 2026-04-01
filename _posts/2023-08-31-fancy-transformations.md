---
title: "Fancy Transformations"
date: 2023-08-31
author: Mauro Ghiani
category: Odds
tags: [CategoryTheory, Functors, FunctionalProgramming, CSharp, Monoids, Options]
excerpt: "In programming languages, a type system is a logical system that comprises variables, expressions, functions, or modules. Since it's clear how Options play in avoiding the use of null, it's natural to ask what happens when we want to add Int32 and Option<Int32>."
---

In programming languages, a type system is a logical system that comprises variables, expressions, functions, or modules. These types, every one in its own, form categories of sets. The main purpose of this system is, of course, to reduce possibilities for bugs defining interfaces and consistency checks.

The reason why we have objects in any category is to mark the origin and the end of morphisms.

Objects and morphisms are both primitive concepts. But somehow morphisms are more important. Tho it required millennia in thoughts and strive for abstraction the idea is very similar to what our ancestors hunting gatherers have been perceived while hunting mammoths.

Since the Stone Age, an indispensable tool for humans was the bow and the arrow. So in the depiction of hunting scenes in caves often the idea of the correlation between objects, the hunter and the prey, is layered down.

But going back to a formal definition, it's convenient remember that a category C = (*Ob(C), Hom(C), dom, cod, ids, []*), requires a collection of *objects*, a collection of *morphisms*, a notion of *domains* and *codomains*, a notion of *identity* morphisms, and a *composition* formula.

Since it's clear how Options (Maybe in Haskell) play in the role of avoiding the use of null in functional language syntax it's natural to ask what happens when we want to add for instance an integer (Int32) and Some integer (Option<Int32>)!!!

As shown above, the operation defined in each category cannot do the trick, we need a Functor!

In fact, for two categories to be comparable, there must a formula that allows the Objects and Morphisms to conform to the following laws: *identities are preserved and composition of morphisms is preserved.*

### What is a functor?

It's an arrow between two categories C and C', that satisfies these properties:

- exists an object F(x) in C' for every object x in C.
- exists a morphism F(f): F(x)-->F(y) in C' for every morphism f: x-->y in C.
- F respects composition, i.e. F(g[f])=F(g)[F(f)] in C' whenever g and f are composable morphisms in C.
- F sends identities to identities, i.e. F(idx)=idF(x) for all objects x in C.

But before going ahead, it is time to compare the category of Int32 and the category of Option<Int32>.

The set of Int32 in .net Core is a (sub)category when we consider the ordinary operation of addition. In particular, it is a monoid, since the sum of integers allows the existence of a neutral element. This neutral element is present in the construction of any identity morphism, so it is called an identity element.

The sum of integers is a binary operation, then Int32 with the ordinary addition + is a monoid:

1) Objects: any integer is an object, ie. 4, 56, -77

2) Morphism: for all x,a in Int32 exists a morphism

3) Composition: + can be composed

4) Associativity: For all *a*, *b*, and *c* in Int32, the equation (a [b]) [c] = a [(b [c])] holds.

5) Identity (neutral) element: There exists an element *e* in Int32 such that for every element *a* in Int32, the equations *e* + *a* = *a* + *e* = *a* hold.

Null objects are nasty things, to deal with them it has been proposed a new type, the option type.

The set of Option<Int32> in .net Core is a (sub)category when we consider a binary operation.

Option<Int32> with the operation of addition <+> is a monoid:

1) Objects: given any integer it's possible to create a type Option<Int32>. Ie. Some 4, Some 56, Some -77, None

2) Morphism: for all o1,o2 in Option<Int32> exists a morphism

3) Composition: <+> can be composed

Function application is *left associative*. That is, evaluating x y z is the same as evaluating (x y) z. And evaluating w x y z is the same as evaluating ((w x) y) z.

For instance using the Function Composition operator (>>), it's possible to compose.

4) Associativity: For all *a*, *b*, and *c* in Int32, the equation (a [b]) [c] = a [(b [c])] holds.

5) Identity (neutral) element: There exists an element None in Option<Int32> such that for every element *a* in Option<Int32>, the equations None + *a* = *a* + None = *a* hold.

For what said before this is a Functor between Int32 and Option<Int32> categories.

The following laws hold:

1. Identities are preserved by F. Also the identity object is preserved, since we are talking about monoids. That is, for any object c in Ob(C), we have F(idc) = idF(c);

2. Composition is preserved by F. Since every morphism is mapped with another between categories. That is, for any objects b, c, d in Ob(C) and morphisms g: b -> c and h: c -> d, we have F(h [g]) = F(h) [F(g)].

One thing should be clear, although, 0 does not map into None but into Some(0).

And finally we arrive to the point, functors that allow adding <Int32> and Option<Int32>. What in Haskell is called fmap.
