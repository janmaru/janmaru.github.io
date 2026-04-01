---
title: "A Gentle Introduction to Category Theory"
date: 2020-05-01
author: Mauro Ghiani
category: Philosophy
tags: [CategoryTheory, Mathematics, Abstraction, FunctionalProgramming, SetTheory]
excerpt: "Category Theory is a branch of mathematics. It's the purest one since it's all about abstraction."
---

**Category Theory** is a branch of mathematics. It's the purest one since it's all about *abstraction*. Well, it should not come as a surprise since many people think that math preexists human intelligence and it lives in the divine realm of ideas. So, if for any reason some particular resistant virus wiped the human race from the face of the earth, euclidean triangles just to say, would be always there to poke their angles into some other alien's eyes. **Math is innate,** the universe is built on its order.

We got the main point in Category Theory: **abstraction**, we want to get rid of all the unnecessary details and look bare at the objects.

Yup, the first important lesson to learn about Category Theory is that we have a bunch/collection of objects. It's not the object in the sense of OOP, so we don't deal with properties or methods, let's say qualities and behaviors. An **object** in Category Theory is primitive. It has no internal structure, it's **nothing**!

*And here comes the story of the Sufi beggar who enters the royal hall while everyone is waiting for the king. The beggar goes straight to the throne and sits on it. Chief Minister alarmed gets close and asks: who do you think you are? Are you a minister?*

*The Sufi answers: I'm much more!*

*So, the Chief says: I'm the Chief of all Ministers, you can't be above me. Are you the King?*

*The Sufi: I'm much more!*

*The Chief: Are you, God, then?*

*No, I'm much more!*

*At this point, the Chief laughs in disbelief and says: there's nothing more than God!*

*And the Sufi teacher says: See... I'm that nothing!*

The first question that arises is this: is it an object an element of a set?

Of course, there could be some Categories where objects are **Sets** or elements of a **Set**, but the definitive answer is **no**!

One of the first notions of Set Naive Theory is that you should always know whether given a Set X and an object a, a belongs to X or not.

```
a ∈ X or a ∉ X
```

By definition the Set X is

```
{ ∀a : 𝒫 = "a satisfies some property" } 
```

so we can say that 𝒫 is also X.

The problem with this approach is the reason why it is called a **naive theory.** It's easy to encounter a **paradox**, for instance, let's create the Set of all Sets that is supposed to contain any Set in the universe along with itself and its Subsets. And this problem was pointed out since the beginning of the theory and easily we can come out with something like the Barber Paradox of Bertrand Russel born in Logic and apply it to Set Theory.

So Category Theory wants to get rid of all these details, like belonging or not, sub belonging, etc. *An object is just what it is.*

The second important lesson to learn about Category Theory is that we have a bunch/collection of **Morphisms** or **Arrows**.

A morphism is a relation between objects.

In the Category is defined a **composition** operation.

This composition of morphisms satisfies the **associative law**.

And is defined, for each object, a particular morphism called **identity.**

So in the end, we can formally define a **category** C as an algebraic structure who is made of the following entities:

- A collection of objects denoted with obj(C).
- A collection of morphisms/relations/arrows/maps between objects, denoted with hom(C). A **morphism** m from source object A to target object B is denoted m: A -> B.
- A **composition** operation of morphisms denoted **[]**. For m1: A -> B and m2: B -> C, their composition is also a morphism m2[m1]: A -> C. Where an order is implied since m2[m1] can be read as m2 after m1.

1. **Associative law**: the composition of morphisms is associative: For m1: A -> B, m2: B -> C and m3: C -> D, there is m3[m2][m1] ≅ m3 [m2 [m1]].
2. **Identity law**: for each object A, there is an identity morphism: ida : A -> A, and also for m: A -> B, there is idB[m] ≅ m ≅ m[idA]

The relevance of Category Theory shines everywhere, from **LINQ** to pure **functional programming**, in the next article we will see an implementation of it imagining the **namespace .net core** as a category of types and functions.
