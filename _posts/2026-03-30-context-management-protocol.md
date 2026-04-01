---
title: "Why You Need a Context Management Protocol"
date: 2026-03-30
author: Mauro Ghiani
category: AI
tags: [ContextManagement, LLMOps, CleanCode, SoftwareArchitecture]
excerpt: "An axiomatic construction cannot simultaneously satisfy consistency and completeness. Here's why hierarchical context files degrade LLM performance and what to do instead."
---

An **axiomatic construction** or **context management** cannot simultaneously satisfy the properties of **consistency** and **completeness**.

### The Shared Configuration Problem

The Git shared configuration problem is **real** and underappreciated. The moment a file enters a shared repository, it stops being configuration and becomes a social **contract** nobody explicitly agreed to. **Noise injection via PR** is genuinely insidious.

The standard hierarchical approach (nested configuration files, global memory, hooks) is **quite problematic**.

It reduces performance on specific projects because it **accumulates** contradictory or context-irrelevant statements through hierarchical nonsense. Files shared via Git by other team members add **certified uncontrolled noise**.

### Why Do You Need a Context Management Protocol?

Any solution devised to simplify the work of developers has proven to be merely an overhead; **convenience** is not a virtue.

For instance, regarding "hooks": they add complexity without providing any clear benefit to any workflow.

Nested configuration files are also a **major source of performance degradation**. Using **automatic** global memory creates too much confusion between projects.

### Why Use a docs/ Structure Instead

Any **docs/** structure intentionally replaces nested and hidden configuration because:

- No **implicit loading**—we know exactly what's in context.
- No loading bugs; the behavior of subfolder configuration is documented but unreliable in practice.
- Zero tokens wasted on **irrelevant** context: if I'm working on a backend issue, I don't load anything in the UI folder.
- Versionable and team-readable: docs/ files are part of the project, not **hidden** configuration.
- The only exception allowed would be a minimalist global configuration (< 30 lines) with stable and universal preferences: response language, commit style, preferred tools—things that never change between projects. However, if you work in too many **different** environments, it's better to avoid it.
- **Agnosticism** should also be preferred over any vendor affiliation.

### The Madness of Hierarchical Context

Consider the typical hierarchy of context files in an LLM-assisted development environment:

- Global user config → always loaded, all projects
- Global local config → not in git
- Project root config → shared project, in git, always loaded
- Project local config → not in git
- Subdirectory config → loaded only on demand
- Modular rules → loaded at launch or per glob pattern
- Auto memory → only the first 200 lines per session

Each layer adds potential for contradiction, noise, and wasted tokens. A single, explicit docs/ structure eliminates this complexity entirely.
