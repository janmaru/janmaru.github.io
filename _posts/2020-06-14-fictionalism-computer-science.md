---
title: "Fictionalism in Computer Science"
date: 2020-06-14
author: Mauro Ghiani
category: Philosophy
tags: [Fictionalism, Platonism, FunctionalProgramming, OOP, CSharp]
excerpt: "Fictionalism in Computer Science opposes the idea that data structures and programs are somehow abstract platonic ideas that can be implemented in any language."
---

Fictionalism in Computer Science opposes the idea that data structures and programs are somehow abstract platonic ideas that can be implemented in any language.

Platonism suggests the view that programs are abstract objects and that the way they are implemented provides a true description of the reality that they are referring to.

For instance, in the platonist way, the sentence "the program A describes a problem or solves a problem" provides a truth independent from the architecture or the language used. So while code is provided, the program A is an abstract object. Such a non-physical object fulfills also some other qualities being non-mental, non-spatial, non-temporal, and non-casual.

For Plato, forms, such as beauty, are more real than any objects that imitate them. So a program A is much more than its architecture and language, for instance, typescript and node js. Its attributes would exist independently and for all other languages!

If a parallel is drawn between natural languages and programming languages some interesting facts can be shown.

For instance, *Shri Bhagwan Rajneesh* recalls that the original Sanskrit word of **meditation** is **dhyana** and he points out that, in the English language, when someone says "meditation", immediately the question arises: "On what?"

Meditation, in the English sense of the word, is always on some object. But in the Sanskrit sense of the word dhyana, there is no object and no subject. Dhyana is empty of all contents. When Buddha's message reached China, there was no equivalent in the Chinese language either. There was no word that could be synonymous with dhyana and for a simple reason: because dhyana was never been practiced anywhere except India. How could such experience being brought up in words?

In China, it was left *untranslated*, but even so, in a different language words change their colors, their forms, their meaning.

Going back to programming languages: C# is an **object-oriented language** and doesn't allow any functions outside of classes. This is a big difference compared to C++ which allows functions to live outside of classes, mainly for backward compatibility with C code. Because functions in C# can only exist within classes, they are typically called methods. But a method it's just behavior, action, it's not an object per se, a primitive as in any functional language.

```csharp
public class NumberManipulator 
{
   public int FindMax(int num1, int num2) 
   {
      int result;

      if (num1 > num2)
         result = num1;
      else
         result = num2;

      return result;
   } 
}
```

This was a random sample found somewhere but it shows the big problem about the platonic view. This class has no other reason to live except the method FindMax. It's tragic but not the real problem. What happens when we decide to add some log into it?

```csharp
public int FindMax(int num1, int num2) 
{
      int result;

      if (num1 > num2)
         result = num1;
      else
         result = num2;

      Console.WriteLine(result);

      return result;
} 
```

The method or function is no more a pure function since its job is not to compute the max between two integers but also to log the result to the console. Now, there's no way it could be placed in a purely **functional environment**.

Not to mention how ugly it is, without using a static method, the creation of an instance of an object only to support a function!

```csharp
var _ = new NumberManipulator();
	   
var result = _.FindMax(3,7);		
```

Platonism is very attractive because it provides a way to abstract the particulars but on the other hand, **Fictionalism** is very very plausible.

And also, it's the least crazy view out there.
