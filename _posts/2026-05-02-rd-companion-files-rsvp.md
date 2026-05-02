---
title: "From .md to .rd — documents that read at the speed of your brain"
date: 2026-05-02
author: Mauro Ghiani
category: Odds
tags: [DocViewer, RSVP, SpeedReading, Markdown, Python, Tkinter, DevTools]
excerpt: "A small filesystem-level pattern: hand-written .rd companion files that play the summary of a Markdown document one sentence at a time, RSVP-style, at configurable speed. The author segments. The tool just reads."
---

> **Scope of this article.** The pattern is platform‑agnostic — it's a sidecar
> file convention plus a tiny RSVP player. The reference implementation
> shown here is wired into **Friedrich**, a Tkinter‑based Markdown viewer,
> but nothing about `.rd` itself depends on Python or the GUI choice.

Most people read at 200–300 words per minute. Trained silent reading pushes
past 400, and techniques like **RSVP** (*Rapid Serial Visual Presentation*)
go beyond 900. At some point the bottleneck is no longer the eye jumping
across a line — it's the brain integrating meaning.

I just added a small pattern to **[Friedrich](https://github.com/janmaru/mahamudra-md-viewer)**
— the Markdown document reader I use for technical docs — that I'm
genuinely fond of: **`.rd` companion files**.

---

## The idea in one line

Next to a `document.md` you can drop a `document.rd`: a hand‑written
summary, **one sentence per line**. The viewer nests it as a child of the
`.md` in the explorer; clicking it opens a dedicated tab that plays the
sentences one at a time, at a configurable speed (200–1200 WPM). No
heuristic splitter, no AI segmentation: cutting the prose into sentences
is the author's job.

```
docs/
  architecture.md
  architecture.rd        ← companion summary
  deployment.md          ← no companion
  loose.rd               ← orphan, opens standalone
```

---

## Why it matters

1. **Summaries become first-class artifacts.** A 20‑page README lives next
   to its 30‑second elevator pitch. Today that pitch is chat, slides,
   email. The `.rd` makes it a file: versioned, diff‑able, sitting next to
   the code.
2. **"Sentence" is the most honest unit.** There's no ambiguity about
   where a chunk of meaning ends — the author declares it explicitly with
   a newline. No generated summary that cuts in the wrong place, no fake
   "TL;DR" that lies.
3. **RSVP changes how you relate to long documents.** Skimming 200
   sentences at 600 WPM is 3–4 minutes of focused attention, vs. 15
   minutes of traditional reading. For onboarding, pre‑meeting refresh,
   weekly alignment, the delta is real.
4. **The pattern is simple.** `<basename>.<ext>` + `<basename>.rd` as a
   sidecar. Same shape as `.md5`, `.gpg`, `.sourcemap`. If another tool
   wants to consume `.rd` tomorrow, all it takes is
   `Path.with_suffix(".rd")`.

---

## Technically

Plain parser (one line = one sentence, inline Markdown only for `**bold**`,
`*italic*`, `` `code` `` as visual emphasis). A Tkinter `RsvpPlayer`
widget takes the list of `Sentence` objects and runs the timer:

```python
duration_ms = max(800, int(words / wpm * 60_000))
```

per sentence. Integration into the existing `TabManager` is one extra
branch:

```python
if is_pdf:
    tab.pdf_viewer = PdfViewer(tab.container, ctx)
elif is_rd:
    tab.rsvp_player = RsvpPlayer(tab.container)
    tab.rsvp_player.load(parse_rd(path))
else:
    tab.html_frame = HtmlFrame(tab.container, ...)
```

The file explorer pairs sidecars on the fly during the scan; orphan `.rd`
files stay top‑level and open the same player.

---

## Edge cases, by convention not by parser

The format stays honest precisely because the parser does almost nothing.
That shifts a small set of conventions onto the author:

- **Lists.** A list is just one‑sentence‑per‑line without bullets. If you
  really need numbering for emphasis, prefix the sentence yourself
  (`"1. First, ..."`) — the leading marker is stripped on render.
- **Abbreviations.** `"Dr. Smith says hello."` is one sentence because
  there's only one newline. The reader never tries to guess.
- **Code‑heavy text.** Wrap snippets in backticks inline; block fences
  are stripped, on purpose, because a full code block doesn't belong in
  an RSVP flow.

These aren't features — they're the consequences of *not* having a smart
splitter, and that's the point.

---

## Scope: summaries first, but the format invites more

I designed `.rd` thinking about **summaries**: the irreducible version of a
document. But the format doesn't really care: it's just a sequence of
sentences. The same file could be a **reading script** for a tutorial, a
**guided tour** of a codebase, a **pre‑flight checklist** before a release.
The viewer doesn't know the difference, and that's a feature: pick the use
that fits, don't ask the format for permission.

---

## Open: per-document pacing

Right now the WPM is user‑driven, set on the slider and remembered as a
preference. A natural next step is **per‑document defaults** — a poem and
a release note shouldn't read at the same speed. The cheapest way to do
it is a one‑line optional header (`wpm: 450`) at the top of the `.rd`,
parsed and stripped before rendering. Open question, not a commitment.

---

## The cultural point, finally

Writing the `.rd` for a document is a discipline exercise: it forces you
to ask *what is the irreducible sentence?*, *what remains if I strip
everything else?*. It's far more useful than it sounds.

And yes — it's also genuinely fun to watch your own document fly past at
800 words per minute.

---

## Code

Source and reference implementation: [github.com/janmaru/mahamudra-md-viewer](https://github.com/janmaru/mahamudra-md-viewer).
