---
title: "Dive Into Claude Code: Permission Fatigue and the Architecture of Trust"
date: 2026-04-20
author: Mauro Ghiani
category: AI
tags: [AgenticAI, ClaudeCode, Anthropic, AIArchitecture, MCP, ModelAlignment, Sandboxing]
excerpt: "Users approve 93% of permission prompts. Anthropic's answer is not more warnings, but sandboxing, classifiers, and a deterministic infrastructure around a simple while-loop."
---

![Dive into Claude Code](/assets/images/dive-into-claude-code.jpg)

A recent technical report, [*Dive into Claude Code: The Design Space of Today's and Future AI Agent Systems*](https://arxiv.org/abs/2604.14228), dissects the TypeScript source of Claude Code and compares it with OpenClaw. The interesting part is not the loop at the center — which is almost trivial — but the infrastructure built around it to make an agent usable without becoming reckless.

### The Permission Fatigue Problem

Anthropic measured what most of us suspected: users approve **93% of permission prompts**. Past a certain threshold, prompting stops being a safety mechanism and starts being noise. The boundary is still there on paper, but cognitively it has already collapsed.

The naive fix is to add more warnings. Anthropic went the opposite direction: **sandboxing** and **automatic mode classifiers** that let the agent operate with less approval friction precisely where friction has become meaningless.

### Four Risk Categories

The design addresses four distinct failure modes:

* **Overeager behavior** — the agent doing more than it was asked.
* **Honest mistakes** — well-intentioned errors inside the agent's own reasoning.
* **Prompt injection** — external content hijacking the agent's instructions.
* **Model misalignment** — the model optimizing for the wrong objective.

These four are not interchangeable. Each one needs a different mitigation, and lumping them together is how you end up with a security theater that does nothing.

### The Three-Phase Loop

At the core, Claude Code runs a **gather context, take action, verify results** loop. This is a *while-loop that calls the model, runs tools, and repeats*. That is the whole engine.

The complexity lives in the scaffolding around the loop:

* A permission framework with **seven modes** and ML-based classification.
* **Context management** via a five-layer compaction pipeline.
* Four extensibility surfaces: **MCP, plugins, skills, hooks**.
* **Subagent delegation** with worktree isolation.
* Append-oriented session storage.

### Deterministic Infrastructure Over Clever Policies

The design bet is explicit: prioritize **deterministic infrastructure** — context management, tool routing, recovery — over constraining decision frameworks imposed on the model. Make the substrate boring and predictable; let the model be the stochastic part.

### Auto-Approval as a Learned Trust Curve

The configurability through `CLAUDE.md`, skills, MCP, hooks, and plugins is not cosmetic. It is how the system learns when *not* to ask. Anthropic reports auto-approval rates scaling from roughly **20% at 50 sessions to 40%+ at 750 sessions**. Trust is not declared — it is accumulated.

### Why This Matters

The paper identifies six open design directions for future agent systems. The larger lesson is architectural: in agentic AI, the model is not where you win or lose. You win or lose in the permission system, the context pipeline, the recovery path — the parts nobody screenshots.

Full paper: [arxiv.org/abs/2604.14228](https://arxiv.org/abs/2604.14228).
