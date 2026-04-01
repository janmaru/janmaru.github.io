---
title: "Beyond Conventions: Why LLM Context Management Requires Explicit Configuration"
date: 2026-04-01
author: Mauro Ghiani
category: LLM
tags: [LLMOps, Architecture, CleanCode]
excerpt: "Context is not just data; it's a finite resource of probability. Here's why we should favor configuration over conventions."
---

In the world of Large Language Models (LLMs), two mutually exclusive events almost cover the entire space of possibilities. Context is not just data; it's a finite resource of likelihood for an event occurring, expressed as a number. In models like Transformers, this context is literally a bounded window where every token competes for weight—acting, in a sense, like an "attention whore" vying for importance in terms of probability.

### The Fallacy of Human-Like Intuition

We often fail to account for how the Transformer architecture actually works when we treat an LLM like a human developer who can "just ignore" irrelevant folders. Unlike humans, the softmax function doesn't care about the nesting of your folders; it only sees a linear sequence of tokens.

When we use a hierarchical "auto-loading" structure, we force the model to navigate tree-based logic through that linear window. For instance, while models like Claude employ fragmented attention maps to cross-reference distant tokens and resolve inheritance, this adds unnecessary computational strain.

### The Power of Physical Adjacency

To maintain coherence effectively, we should aim to create "dense" attention blocks. By placing the most relevant constraints physically adjacent to the task, we reduce the model's internal struggle.

In a hierarchical setup, when a sub-directory configuration (like `.clauder`) conflicts with a root instruction (like `CLAUDE.md`), the model's internal hidden states are essentially fighting. In a system with two dominant conventions, context almost completely determines which one will emerge.

### The Probability of "Dirty" States

Consider the equation:

`P(tabs | context) + P(spaces | context) ≈ 1`

Why "≈ 1" and not exactly 1? Because we aren't dealing with a perfect distribution. We are dealing with noise: "dirty" states, mixed files, failed automatic conversions, poorly resolved merge conflicts, or legacy code.

### The Solution: A Single Source of Truth

This is why we should favor **configuration over conventions**. By moving agent context management to an explicit structure (such as a `/docs/context.md` file), we perform prompt engineering at a higher level.

An explicit source of truth ensures that the probability mass is concentrated on the correct tokens. In a well-formed context, this concentration becomes so strong that one convention dominates, ensuring the model remains focused, coherent, and efficient.
