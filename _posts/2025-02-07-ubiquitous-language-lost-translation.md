---
title: "Ubiquitous language: Lost in translation"
date: 2025-02-07
author: Mauro Ghiani
category: Philosophy
tags: [DDD, DomainDrivenDesign, UbiquitousLanguage, Semantics, Saussure, BoundedContext]
excerpt: "Ubiquitous language is an idea commonly used in Domain-Driven Design. It refers to a shared vocabulary between team members, but its true power lies in the shared meaning, or semantics, that it carries."
---

**Ubiquitous language** is an idea commonly used in Domain-Driven Design (**DDD**).

*It refers to a shared **vocabulary** between team members, so speaking: developers, business analysts, domain experts, and anyone else involved in the project.*

The idea is to create a language specific to the domain and ready to evolve alongside the domain model, reflecting the terms, processes, and concepts relevant to the business problem.

By using the **same terms** in conversations, documentation, and code, everyone ensures a deeper understanding of the system.

Ubiquitous language might initially seem like just a **vocabulary** or lexicon, but its true power lies in the shared meaning, or **semantics**, that it carries. The **semantic** layer is built collaboratively and contextually by the team, especially domain experts. The meaning of each term in the ubiquitous language is refined and defined through *discussions, modeling, and interaction with the domain.*

In a DDD context, semantics are given to developers and domain experts (like business stakeholders) who engage in constant contact to align the terms with real-world concepts and business needs.

For instance using **Event storming,** a workshop-based method to quickly find out what is happening in the domain.

Semantics, that is, the meaning behind the terms in a ubiquitous language (UL) are part of the domain model itself.

For instance, an *"order is emitted,"* if we think of "order" as a term in the ubiquitous language shared across various parts of the domain, the question is: *Who determines the specific meaning, the semantics, of that term?* Is it part of the model, or is it something separate that people just agree on?

To make it clear, **the Vocabulary (UL) is shared across the domain, so th**e *vocabulary* gives the names or terms for various concepts, but those terms are meaningless without a deeper understanding of how they map to actual processes or entities within the domain.

The *semantics* of a term (like "order") are defined within the context of the domain model. The domain model is where you define how an "order" behaves, what properties it has, and how it interacts with other parts of the system.

In the Sales context might be a request made by a customer to purchase products. It could have properties like order date, customer, items, etc...

This semantics defines what an "order" **actually** is and what it means within that specific **bounded context.**

The order is placed.

In this case, "placed" could be part of the semantics in the Sales model. For example, an order could be "placed" once it has been confirmed, meaning it has been processed or sent to another part of the system (such as inventory or shipping).

This action or change in state is part of the behavior described in the domain model.

The **semantics** of terms in the ubiquitous language is indeed part of the **domain model**, but it is context-specific.

The term "order" has a general meaning, but its full, precise semantics depend on where it's being used (e.g., in Sales, in Shipping, etc.), and the **model** defines those meanings, rules, and behaviors for each bounded context.

The term "order" might mean something different in *Shipping* than it does in *Sales*, and the model in each context defines what that term represents, how it behaves, and how it relates to other concepts. Does that clarify things a bit more?

S**emantic** is not just about agreeing on words.

It is about ensuring that everyone in each bounded context shares a consistent **meaning** for those words and that the model correctly represents the domain.

Without semantics, a **vocabulary** becomes just a set of arbitrary labels, and the **Ubiquitous Language** becomes ineffective.

*We could go even further, thinking of a ubiquitous language (UL) as a form of modeling, but with some important distinctions.*

In a broad sense, **modeling** refers to creating a structured representation of the domain to understand, communicate, and manage its complexities.

The **ubiquitous language** can be seen as the **linguistic representation** of that model. In other words, the terms and concepts in the UL are reflections of the domain model. As the model evolves, so does the language.

The **semantics** give the **words** meaning, but also **context and behavior**. In UL, the **semantic layer** is what transforms a simple vocabulary into a powerful communication tool. It's not just about what terms are used (the vocabulary), but about **how they are understood** within the **model** and how they map to actual **domain concepts**.

**Modeling without semantics** would mean having structures and definitions (e.g., entities, relationships) but not truly understanding their **meaning** or how they relate to the real world. The semantics tie those abstract models to the **real-world context** they represent, making them relevant and actionable.

In summary, **semantics are the heart of any language,** whether natural or technical, because they provide the **meaning** that allows the language to communicate **understanding**. Without semantics, language would lose its purpose. In UL, the semantics are what ensure the shared vocabulary is actually meaningful and useful for building a shared understanding of the domain.

But if you have a vocabulary, as **Saussure** says, the connection between the **signifier** and the **signified** is arbitrary, when you define something universal without "specific" words it's completely useless, of course, we agreed on "order" instead of "Donald duck" but if the language does not convey the meaning in sales, it's completely useless.

We could have easily used a completely different word, like "donald duck," to signify the same thing (like "order") if everyone in the community agreed to it. This is the core of **linguistic relativity**: the meaning of words is determined by social convention.

So, when we move to something like **ubiquitous language (UL)** in **Domain-Driven Design (DDD)**, the importance lies in **shared agreement** and **context**, the **semantic meaning** comes from the **shared understanding of the terms**, not just their arbitrary selection.

The **context** in which the language is used brings those connections to life, in the case of **"order"** in **Sales**, it's not just a random word; it's a concept **agreed upon** by all stakeholders to represent something specific in the business domain.

The **semantics** of "order" don't just come from the word itself, they are shaped by how it's **defined in the business context**, how it's **modeled**, and how it fits into the overall system.

The **language** (like Saussure's theory) is arbitrary, but when it's **contextualized** within a specific domain, its semantics are **created** through the shared understanding of the stakeholders involved. The **real value** of a **shared vocabulary**, like the term "order", comes from how it's tied to a **specific meaning** in the **Sales domain**. Without that connection, the language becomes just noise. So, yes, a vocabulary without the semantic context **is completely useless** in the domain, and it's the **shared model and meaning** that gives that vocabulary power and utility.

T**he meaning of "order" is not universal across the entire domain** but is instead **specific to each bounded context**.

**Sales** defines what "order" means within its own context, and that meaning is based on the specific **rules, processes, and behaviors** of that domain.

For **Sales**, "order" could refer to a request made by a customer for products, including properties like customer information, items, and total cost. However, in **Shipping**, the term "order" could be understood completely differently, possibly referring to the process of preparing and dispatching goods based on the order that was placed in the Sales context.

If we could **share semantics** across bounded contexts, we could *potentially* have a much richer, **unified** language that communicates the meaning of terms across the system. This would ideally create a more coherent and consistent experience when dealing with different parts of the system.

A **vocabulary** is just a collection of words or terms, but **language** is more than that, it's the **system of meaning** behind those words, the **rules** governing their use, and the **context** in which they're applied. So, when we talk about a **Ubiquitous Language (UL)** in **Domain-Driven Design (DDD)**, it's not just about having a shared vocabulary, it's about having **shared meanings** that everyone in the domain (business experts, developers, etc...) understands and aligns with.

To create a true **language**, **semantics** must be shared across the **bounded contexts**, and the **modeling** within each bounded context must reflect that shared understanding.
