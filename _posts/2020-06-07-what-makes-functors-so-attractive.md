---
title: "What makes functors so attractive"
date: 2020-06-07
author: Mauro Ghiani
category: Philosophy
tags: [CategoryTheory, Functors, FSharp, Haskell, Monoid, OptionType, FunctionalProgramming]
excerpt: "For two categories to be comparable, there must be a formula that allows the Objects and Morphisms to conform to the following laws: identities are preserved and composition of morphisms is preserved."
---

In programming languages, a **type system** is a logical system that comprises variables, expressions, functions, or modules. These types, every one in its own, form categories of sets. The main purpose of this system is, of course, to reduce possibilities for bugs defining **interfaces** and consistency checks.

The reason why we have objects in any category is to mark the origin and the end of morphisms.

Objects and morphisms are both **primitive** concepts. But somehow morphisms are more important. Tho it required millennia in thoughts and strive for abstraction the idea is very similar to what our ancestors hunting gatherers have been perceived while hunting mammoths.

Since the Stone Age, an indispensable tool for humans was the bow and the arrow. So in the depiction of hunting scenes in caves often the idea of the correlation between objects, the hunter and the prey, is layered down.

But going back to a formal definition, it's convenient remember that a **category C** = (*Ob(C), Hom(C), dom, cod, ids, []*), requires a collection of *objects*, a collection of *morphisms*, a notion of *domains* and *codomains*, a notion of *identity* morphisms, and a *composition* formula.

Since it's clear how **Options** (Maybe in Haskell) play in the role of avoiding the use of null in functional language syntax it's natural to ask what happens when we want to add for instance an integer (**Int32**) and Some integer (**Option<Int32>**)!!!

```fsharp
let add''' x y = x + y 
printfn "%A + %A  = %A" a b  (add a b )
printfn "%A + %A = %A" 1 3  (add''' 1 3)


Some 1 + Some 3  = Some 4
1 + 3 = 4



printfn "%A + %A = %A" a 3  (add 1 3)


This expression was expected to have type
    'int option'    
but here has type

    'int' 
```

As shown above, the operation defined in each category cannot do the trick, we need a **Functor**!

In fact, for two categories to be **comparable**, there must a formula that allows the Objects and Morphisms to conform to the following laws: *identities are preserved and* *composition of morphisms is preserved.*

### What is a functor?

It's an arrow between two categories C and C',

```
F: C -> C'
```

that satisfies these properties:

- exists an **object F(x)** in C' for every object x in C.
- exists a **morphism F(f): F(x)->F(y)** in C' for every morphism f: x->y in C.
- F respects **composition**, i.e. F(g[f])=F(g)[F(f)] in C' whenever g and f are composable morphisms in C.
- F sends **identities** to identities, i.e. **F(idx)=idF(x)** for all objects x in C.

But before going ahead, it is time to compare the category of **Int32** and the category of **Option<Int32>.**

The set of Int32 in .net Core is a (sub)**category** when we consider the ordinary operation of addition. In particular, it is a **monoid**, since the sum of integers allows the existence of a neutral element. This neutral element is present in the construction of any identity morphism, so it is called an identity element.

The sum of integers is a binary operation defined as:

```
Int32 x Int32 -> Int32
```

then Int32 with the ordinary addition + is a monoid:

1) **Objects**: any integer is an object, ie. 4, 56, -77

2) **Morphism**: for all x,a in Int32 exists a morphism

```
Ma(x)=x+a
```

3) **Composition**: + can be composed in this matter

```
Ma+b(x)=(Mb[Ma])(x)
```

In fact:

```
Ma+b(x) = x+(a+b) By the definition of M   

        = (x+a)+b Associativity       

        = (Ma(x))+b By sostitution

        =  Mb(Ma(x)) Definition of M by b

        = (Mb[Ma])(x) Definition of Composition of Mappings   
```

4) **Associativity:** For all *a*, *b*, and *c* in Int32, the equation (a [b]) [c] = a [(b [c])] holds.

5) **Identity (neutral) element:** There exists an element *e* in Int32 such that for every element *a* in Int32, the equations *e* + *a* = *a* + *e* = *a* hold.

Null objects are nasty things, to deal with them it has been proposed a new type, the **option type**.

The set of **Option<Int32>** in .net Core is a (sub)category when we consider the following binary operation.

```fsharp
let (<+>) a b =
    match (a, b) with
    | (Some x, Some y) -> Some (x + y)
    | (Some x, None)   -> Some (x)
    | (None, Some y)   -> Some (y)
    | (None, None)     -> None
```

In fact, given a,b and c:

```fsharp
let a = Some 1
let b = [1;2;3;4]  |> List.tryFind (fun x-> x = 3)  // Some 3
let c = [1;2;3;4]  |> List.tryFind (fun x-> x = 10) // None
```

And redefining the name of the morphism:

```fsharp
let add o1 o2 =  o1 <+> o2
```

It's possible to print out the results:

```fsharp
printfn "%A + %A = %A" a b (add a b)
printfn "%A + %A = %A" a c (add a c)
printfn "%A + %A = %A" c a (add c a)


Some 1 + Some 3 = Some 4
Some 1 + None = Some 1
None + Some 1 = Some 1
```

**Option<Int32>** with the operation of addition **<+>** is a **monoid**:

1) **Objects**: given any integer it's possible to create a type Option<Int32>. Ie. Some 4, Some 56, Some -77, None

2) **Morphism**: for all o1,o2 in Option<Int32> exists a morphism:

```fsharp
add o1 o2 =  o1 <+> o2
```

3) **Composition**: <+> can be composed in this matter:

```fsharp
let add' o1 o2 o3 =  (o1 <+> o2) <+> o3
Some 1 + Some 3 + None = Some 4
```

Function application is *left associative*. That is, evaluating x y z is the same as evaluating (x y) z. And evaluating w x y z is the same as evaluating ((w x) y) z.

For instance using the **Function Composition** operator (**>>**):

```fsharp
let inline (>>) f g x = g(f x)
```

It's possible to compose:

```fsharp
 let f x = add x a
 let g z = add z b
 
 let fg = g >> f  
 
 printfn "%A" (fg c)

 Some 4
```

4) **Associativity**: For all *a*, *b*, and *c* in Int32, the equation (a [b]) [c] = a [(b [c])] holds.

```fsharp
let add' o1 o2 o3 =  (o1 <+> o2) <+> o3
let add'' o1 o2 o3 =  o1 <+> (o2 <+> o3)


printfn "%A + %A + %A = %A" a b c (add' a b c)
printfn "%A + %A + %A = %A" a b c (add'' a b c)


Some 1 + Some 3 + None = Some 4
Some 1 + Some 3 + None = Some 4
```

5) **Identity** (neutral) element: There exists an element **None** in Option<Int32> such that for every element *a* in **Option<Int32>**, the equations None + *a* = *a* + None = *a* hold.

```fsharp
printfn "%A + %A = %A" None b (add None b)
printfn "%A + %A = %A" b None (add b None)


None + Some 3 = Some 3
Some 3 + None = Some 3
```

For what said before this is a Functor between **Int32** and **Option<Int32>**: categories.

```fsharp
let opt (a:int) = Some(a)
```

for instance:

```fsharp
printfn "%A -> %A" 3 (opt 3)

3 -> Some 3
```

The following laws hold:

1. **Identities** are preserved by F. Also the identity object is preserved, since we are talking about monoids. That is, for any object c in Ob(C), we have F(idc) = idF(c);

```fsharp
printfn "%A + %A = %A -> %A + %A = %A" 3 0 (3+0) (opt 3) (opt 0) ((opt 3) <+> (opt 0))

3 + 0 = 3 -> Some 3 + Some 0 = Some 3
```

2. **Composition** is preserved by F. Since every morphism is mapped with another between categories. That is, for any objects b, c, d in Ob(C) and morphisms g: b -> c and h: c -> d, we have F(h [g]) = F(h) [F(g)].

```fsharp
printfn "%A -> %A = %A + %A -> %A  + %A = %A" (3 + 4) (opt (3 + 4)) 3  4  (opt 3)  (opt 4) ((opt 3) <+> (opt 4))

7 -> Some 7 = 3 + 4 -> Some 3  + Some 4 = Some 7
```

One thing should be clear, although, 0 does not map into None but into Some(0).

And finally we arrive to the point of the article, functors that allow adding <Int32> and Option<Int32>. What in Haskell is called fmap:

```haskell
class Functor f where
    fmap :: (a -> b) -> f a -> f b
    (<$) :: a -> f b -> f a
```

where:

```haskell
fmap id = id
```

and also:

```haskell
fmap (f . g)  ==  fmap f . fmap g
```

It is left to the attentive reader the proof that the arrow so defined is a functor.
