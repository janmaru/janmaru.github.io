---
title: "You're good as anyone else"
date: 2020-05-03
author: Mauro Ghiani
category: Philosophy
tags: [CategoryTheory, CSharp, Identity, LINQ, ExpressionTrees]
excerpt: "In Category Theory identity is just an arrow that starts from the object and ends to it. Somehow the object and the subject are the same."
---

Nature is working on creating unique individuals, whereas culture has invented a single blueprint to which all people should conform. It is madness. The problem with **identity** is that while you spend time creating a "persona," a mask, at the very core nothing is there. Identity's not a substitute for your individuality, that's why it so hard to acquire and so hard to maintain.

In Category Theory identity it's just an arrow that starts from the object and ends to it. Somehow the object and the subject are the same. And today we start with a simple scenario. We want to order all natural numbers. Ok, at least those defined as Int32, so that we start from int.MinValue and we end at int.MaxValue. The relation of order is "**less than or equal to**", or in symbols **<=**.

It's clear that we have constructed a simple Category, *a <= b* is a **Morphism** between numbers (i.e. 2<=3) and composition, identity, and associative law are enforced.

How can we represent in C# a category? We could use an interface.

The **objects** are a collection that exposes an enumerator, which supports a simple iteration over a non-generic collection. The reason is clear: can we imagine to represent an infinite set of values, like all the natural numbers with the finite resource of a computer?

C# cannot speed the creation of the objects since it has just an iterator. If we consider the Category of the **Int32** in .net world we could have:

An expression represents a strongly typed lambda expression. The *.net compiler* converts the lambda expression which is assigned to Expression<TDelegate> into an **Expression Tree** instead of executable code. This expression tree is used by remote LINQ query providers as a data structure to execute logic out of it.

A Morphism is defined as:

```
Y <= Z
```

The composition requires only two binary expressions, for instance:

```
(Y <= Z) [(X <= Y)] => X <= Z.
```

Also identity it's a morphism of an object on itself.

Unit testing is quite easy and forthcoming since the **compile** method can be used to convert a Lambda Expression tree into the delegate that it represents.

Also **.ToString()** can be used to return the whole expression as a string.

Finally, we have shown that the relevance of **Category Theory** shines everywhere, from **LINQ** to pure *functional programming.*
