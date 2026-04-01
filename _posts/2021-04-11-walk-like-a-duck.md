---
title: "Walk like a duck"
date: 2021-04-11
author: Mauro Ghiani
category: Philosophy
tags: [CategoryTheory, Monoid, FSharp, CSharp, DuckTyping, Strings]
excerpt: "Are numbers objects or arrows, behavior or data? The most interesting application of monoid does not relate only to numbers but strings."
---

When Emil Mazey, secretary-treasurer of the United Auto Workers, at a labor meeting in 1946 pointed at someone accusing him of being a communist, he said:

"I can't prove you are a Communist. But when I see a bird that quacks like a **duck**, walks like a duck, has feathers and webbed feet, and associates with ducks -- I'm certainly going to assume that he *is* a duck."

Emil used a form of reasoning in which it is necessary to investigate an unknown problem by observing similar characteristics that occur in other cases. It's called **the duck test**.

Let's say that integers have values and functions also. Integers are sold as actions, for instance, sums. Also, you can throw them with dice. What are you going to conclude?

Are **numbers** objects or arrows, behavior or data?

Normally numbers are objects, the element of a set N ={ ...-1,0,1,2,3...}.

But a number is also a morphism, for instance, the number one: 1 -> 1.

Neumann showed that each ordinal is the well-ordered set of all smaller ordinals.

0 = { } = empty set

1 = { 0 } = {empty set}

2 = { 0, 1 } = { empty set, {empty set} }

3 = { 0, 1, 2 } = { empty set, {empty set} , {empty set, {empty set}} }

4 = { 0, 1, 2, 3 } = { empty set, {empty set} , {empty set, {empty set}}, {empty set, {empty set}, {empty set, {empty set}}} }

But even if "most mathematicians prove what they can, von Neumann proves what he wants," (1) he could not see the most generic abstraction of category theory.

The most interesting application of monoid does not relate only to numbers (int, long, short, byte) but string(s).

A "string" represents **texts** as a sequence of UTF-16 code units, so it's a **sequential** collection of **characters**.

For instance, in C#:

```csharp
char[] chars = { 'w', 'o', 'r', 'd' };
sbyte[] bytes = { 0x41, 0x42, 0x43, 0x44, 0x45, 0x00 };


string stringFromBytes = null;
string stringFromChars = null;
unsafe
{
   fixed (sbyte* pbytes = bytes)
   {
      // Create a string from a pointer to a signed byte array.
      stringFromBytes = new string(pbytes);
   }

   fixed (char* pchars = chars)
   {
      // Create a string from a pointer to a character array.
      stringFromChars = new string(pchars);


   } 
}
```

In F# we can use a module to represent a monoid.

```fsharp
namespace Mahamudra.Functions
open System


module Monoid = 
    /// the identity arrow
    let identity = String.Empty 


    /// composition law
    let ( >> ) a b = a + b


module Test =
    open Monoid
 
    let string = "yup" 


    /// is the identity satisfied?
    printfn "Is the string %A >> with identity %A equal to the string itself %A" 
string  identity (string >> identity) 
   
    /// associative law
    let a = "this is "
    let b = "a "
    let g = "sentence "


    printfn "a=%A, b=%A, g=%A // (a >> b) >> g = a >> (b >> g) %A >> %A = %A >> %A // %A=%A"  
a b g (a>>b) g a (b>>g)  ((a>>b)>>g) (a>>(b>>g)) 
```

And finally the output:

Is the string yup >> with identity equal to the string itself yup

a=this is , b=a , g=sentence // (a >> b) >> g = a >> (b >> g) this is a >> sentence = this is >> a sentence // this is a sentence =this is a sentence

(1) a popular saying among mathematicians of his day.
