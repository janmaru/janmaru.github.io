---
title: "AI Code Detection: Eight Heuristics, One Honest Score"
date: 2026-04-16
author: Mauro Ghiani
category: AI
tags: [AIDetection, CodeReview, Heuristics, OpenSource, Python]
excerpt: "A heuristic-based detector that scores AI involvement in code — comment density, fingerprinting, commit patterns, and more. Honest about its 55-65% precision."
---

I built [mahamudra-ai-code-detector](https://github.com/janmaru/mahamudra-ai-code-detector), a Python tool that analyzes git repositories to estimate where AI coding assistants were likely involved in writing or editing code. It is intended as an **aid for code review and auditing** — not as forensic proof of authorship.

The detector utilizes eight distinct heuristics to identify patterns characteristic of AI-generated code. It generates a final probability score (0 to 1) based on a weighted scoring formula. However, the system is purely heuristic-based and lacks semantic understanding, resulting in a precision range of **55-65%**.

### Comment Analysis (Strongest Signal)

This is the most reliable indicator of AI involvement.

* **Density:** AI models (Copilot, Claude) typically produce **25-40%** comment density, whereas humans average **5-15%**. The system flags files exceeding **30%**.
* **Phrasing:** Detects "instruction-following" language (e.g., *"This function does X"*, *"Here we initialize Y"*).

### Fingerprinting & Boilerplate Detection

* **Method:** Code is broken into 5-line chunks, normalized (no whitespace/comments), and hashed.
* **Red Flags:** 30% or more of the code consists of repeated lines. Multiple code blocks are 80% similar to each other.
* **Logic:** AI tools often repeat specific boilerplate patterns across different files.

### Commit & Development Patterns

The system monitors how code is introduced to the repository:

* **Commit Size:** Large commits (>500 insertions) are flagged.
* **Burst Activity:** Multiple large changes from one person within a single week.
* **One-Shot Generation:** Pull requests where changes occur in a single commit rather than through iterative development.

### Code Consistency (Weaker Signal)

* **Logic:** Looks for perfectly uniform indentation and naming conventions.
* **Flaw:** The detector assumes human code is inconsistent; however, modern AI tools often adapt to existing styles, making well-formatted code a potential "false positive" indicator.

### Bot Signature Detection

* **Method:** Scans author names, emails, and "co-authored-by" trailers.
* **Effectiveness:** Very low (~5% catch rate). AI tools like Cursor or Claude run locally and do not automatically list themselves as authors.

### System Limitations

* **Surface-Level Analysis:** The system relies on patterns, not logic. It can be fooled by verbose human writers or terse AI prompts.
* **Weighting Imbalance:** Currently, the system weights bot signatures most heavily despite comment analysis being the most reliable metric.
* **Reliability:** Useful for flagging suspicious code for manual review, but insufficient for definitive proof of AI use.

The full source, CLI, and desktop GUI are available on [GitHub](https://github.com/janmaru/mahamudra-ai-code-detector).
