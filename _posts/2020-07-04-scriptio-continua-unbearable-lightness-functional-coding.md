---
title: "Scriptio Continua and the Unbearable Lightness of Functional Coding"
date: 2020-07-04
author: Mauro Ghiani
category: Philosophy
tags: [FunctionalProgramming, NaturalLanguage, FSharp, CSharp, LINQ, Expressiveness]
excerpt: "Natural languages and programming languages are often compared for a simple reason: they both try to define some meaning and the latter also procedural steps to achieve some goal."
---

Natural languages and programming languages are often compared for a simple reason: they both try to define some meaning and the latter also procedural steps to achieve some goal.

### Natural Languages

Also, natural languages can be used to define a list of paces, the typical example being a todo list or a recipe. But a natural language has great limitations, it is:

1. **Ambiguous;**
2. **Redundant;**
3. **Recursive;**
4. **Arbitrary;**
5. **Growing;**

That's why we have poetry and literature!!!

*The Sick Rose*, written by William Blake, is abundant of ***ambiguities***:

```
O Rose thou art sick.

The invisible worm,

That flies in the night

In the howling storm:

Has found out thy bed

Of crimson joy;

And his dark secret love

Does thy life destroy
```

What does "*dark secret love*" mean? And "*bed of crimson joy*"?

When natural languages mix up with images the ambiguity of the message can be even highlighted. In that case, only ***the context*** can shed a light on the real meaning of the text.

***Redundancy*** is the needless repetition of words, sentences, or ideas. For example, "*the reason why*", "*my past experience*", "*early beginnings*, *merge together*" etc... Often, redundancies occur in speech accidentally, but redundant phrases can also be deliberately constructed for emphasis.

Linguist Noam Chomsky has argued that the lack of an upper bound on the number of sentences and their length it's the base of **recursion** in every natural language.

A verb in a sentence can be followed by another sentence: "John thinks pizza is really good", in which the sentence "pizza is" occurs after the other. So a sentence can be defined recursively by a noun phrase, a verb, and optionally another sentence.

A visual analogy would be the Droste effect, a known form of recursion in painting.

The last quality of NL is arbitrarity. A language is an **arbitrary**, cultural construct. So there is no basis for relating natural languages to other aspects of human biology or evolution.

### Growing

Understanding human language from the point of a machine is quite hard. Today NLP (natural language processing) uses machine learning to understand the speech of humans. Alexa and Google Assistant are a very good example of that.

When the skill or action created by programmers encounters a new word it's needed to train the dataset. In other words, like for humans, machines learn how to deal with new vocabulary. So the right spelling and pronunciation should be enforced upon the NLP engine.

### Programming Languages

A Programming Language might be instead:

1. **Productive;**
2. **Expressive;**
3. **Linear;**

### Productive

In the beginning, programming languages were designed to be easily interpreted by machines, but not people. By the time this changed, today spaces, tabs, scopes are used only to make the code more **readable** to humans!

Programming does allow some variation in style, but the meaning is not flexible. At the end of the day, any higher language is translated in byte code for the machine execution.

### Expressive

Functional languages are expressive and use short, succinct, and readable code. They hide *how* the code executes and through rich abstractions, can hide all the complexity of code.

F.P. makes programs readable and easy to discuss, thus making it possible to understand and change previously unknown code.

Functional programming uses a declarative programming style and it expresses the logic of programs without specifying the execution details.

Traditionally, documentation of a software program involves a mix of code and natural language.

Donald Knuth was the first to propose a method of literate programming, where the primary document is prose that follows the human logic of the programmer.

Normally we have instead a natural language that describes code, like the Javadoc tool, where separate code and N.L. are merged into one document by the tool (i.e a tutorial.)

But there are also samples of functional programming languages that are so expressive that the code is also language!

F# is so clear that it can be read as prose itself.

```fsharp
module CardGame =

    type Suit = Club | Diamond | Spade | Heart
    
    type Rank = Two | Three | Four | Five | Six | Seven | Eight | Nine | Ten | Jack | Queen | King | Ace

    type Card = Suit * Rank

    type Hand = Card list

    type Deck = Card list

    type Player = { Name:string; Hand:Hand}

    type Game = {Deck:Deck; Players: Player list}

    type Deal = Deck -> (Deck*Card)


    type PickupCard = (Hand*Card) -> Hand
```

Another clear example of expressiveness is Linq.

Linq is defined as a mean to provide a language-level querying tool and a higher-order function API to C# as a way to write expressive, declarative code.

For instance, the query syntax:

```csharp
var linqExperts = from p in programmers
                  where p.IsNewToLINQ
                 
                  select new LINQExpert(p);
```

Or using the API syntax:

```csharp
var linqExperts = programmers.Where(p => p.IsNewToLINQ)
                  
                             .Select(p => new LINQExpert(p));
```

Well, there is -also- the ivory tower. The CEO says: "*If it weren't for the pesky consumer, our business would be good.*"

It's a magical, chic place where all the programming languages live!

On the top, only pure functional languages are placed... well, why? Because they're cool, of course. And also because they are much more **expressive** then the others, like Visual Basic for instance.

### Linear

When did English (and other languages) first start to use spaces between words? And do all languages use spaces to mark divisions between words?

Today, in modern Japanese, when ideograms are used in writing, even if they are placed in a continuum, the document is easy to read.

Linear B samples have much space between phrases, probably for this reason.

Ancient Greek, which came after Linear B, after a while became a stream of letters: a left-to-right language, then sometimes right-to-left or words were placed in alternate rows. The continuum is thought to be "hard" to read, as our brain is not much different from the brain of ancient Greeks. But the "script" is also thought to be read aloud, and so people would read the document before speaking. Also, some kind of imaginative and creating reading would occur. The reader would embellish the document while browsing the words.

Like for Ancient Greek, also in Computer Science - at the beginning - information would be presented in a continuum form of data.

But, by saving the reader the tedious process of defining breaks, the inclusion of spaces facilitates the comprehension of the written text.

Moreover, students have a greater capacity to synthesize the text and memorize it.

And this goes with, as an analogy, for each functional language and its expressiveness, compared to imperative programming languages.
