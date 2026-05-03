---
title: "rich-ui: the planet that wasn't a disc"
date: 2026-05-03
author: Mauro Ghiani
category: Odds
tags: [RichUI, Python, TUI, Tkinter, DSL, DataVisualization, Unicode]
excerpt: "In a tiny scatter chart, Earth was rendered as ⊙ — a circled dot, not a disc. Tracking down why leads through two renderers, one DSL, and a contract that's only obvious in hindsight."
---

![scatter_2d before / after — Earth as ⊙ vs ●](/assets/images/rich-ui-planet-marker.svg)

Looking at the **`scatter_2d`** demo in [`rich-ui`](https://github.com/janmaru/mahamudra-rich-ui), I was bothered by something small: in the Voyager example, **Earth at the center wasn't a filled disc**. It was a circled dot — a ring with a small dot inside. Visually wrong: a planet should be a planet.

Tracing back why made me look at a design choice in the library that turns out to be interesting on its own — and a small lesson about feeding **one DSL to two renderers**.

---

## What rich-ui does

`rich-ui` translates a JSON DSL into [`rich`](https://github.com/Textualize/rich) renderables. A `scatter_2d` block expects, in the bound data, a `center` and a list of `tracks`, each carrying `x, y, z` samples and a `current` point. Producer-side, two visual hints exist on every point owner:

- `marker` — a single Unicode glyph
- `shape` — a token from a closed vocabulary (`circle`, `ring`, `ringed`, `diamond`, `triangle_up`, `triangle_down`, `square`, `cross`, `star`, `dot`)

So far so harmless. The problem is that `marker` and `shape` **don't go to the same renderer**.

---

## Two renderers, two contracts

![marker vs shape per renderer](/assets/images/rich-ui-renderer-split.svg)

`rich-ui` has two output paths:

1. **Terminal (TUI)** — the default. ASCII grid, one `rich.Text` glyph per cell. Hard constraint: a terminal cell is one codepoint wide.
2. **Native viewer (Tk)** — a sidecar `tk_scatter_viewer.py` that pops a Tkinter window and draws the same data on a real canvas, with anti-aliased shapes.

The contract is split:

| Field    | Terminal renderer | Native Tk viewer        |
|----------|:------------------:|:------------------------:|
| `marker` | yes — drawn as-is  | legend hint only         |
| `shape`  | **no — ignored**   | yes — controls geometry  |

Reason it ended up this way: a Unicode glyph in a terminal cell is the most you get; if you want a *real* filled disc with a small inner ring (a "ringed" planet), you need pixels, not glyphs. So the TUI honors `marker`, the Tk viewer honors `shape`, and the DSL carries both because the producer doesn't know in advance which target the consumer will pick.

---

## The Voyager fetcher

In `examples/voyager/fetchers/horizons.py`, the trajectory payload's center looked like this:

```python
"center": {
    "label":  "Earth",
    "marker": "\u2299",   # ⊙
    "style":  "bright_blue",
    "shape":  "ringed",
}
```

Two things to notice:

- `marker = "\u2299"` is **U+2299 — `CIRCLED DOT OPERATOR`**. Visually, a ring with a dot inside. It looks circle-ish at a glance, but it is not a filled disc.
- `shape = "ringed"` is documented as *"outer ring with a small inner filled disc — planet-like"*.

So Earth was being rendered as:

- **In the TUI**: literally `⊙`. Hollow ring with a tiny dot. Not a disc.
- **In the Tk viewer**: a `ringed` planet figure. Outer outline, inner dot. Also not a disc.

In both renderers the center was *intentionally* not a filled disc. That intent was buried in the marker choice on one side, and in the `shape` vocabulary on the other.

---

## The fix

For the TUI, change one byte:

```diff
-    "marker": "\u2299",
+    "marker": "\u25cf",
```

`\u25cf` is **U+25CF — `BLACK CIRCLE`**. Filled disc, fits in one terminal cell, renders the same in any monospaced font with BMP coverage.

The `shape` stays `"ringed"`, because in the Tk viewer the original intent — *"render Earth as a planet-like figure"* — is still meaningful and pixel-accurate. A renderer with sub-cell resolution can afford the metaphor; a terminal cannot.

---

## Why this is interesting

When one DSL feeds heterogeneous renderers, **fields stop being interchangeable**. The producer is making two coordinated choices: one for the lossy renderer, one for the rich one. The DSL documentation has to make that explicit, and the producer has to set both fields with their target in mind.

Three small principles fall out of this:

1. **Make the contract explicit in the DSL reference.** A single line — *"`shape` is consumed by the native viewer; the TUI uses `marker` only"* — prevents the trap. `rich-ui` documents this in `DSL_REFERENCE.md`, and that's why I could find the answer in five minutes instead of an hour.
2. **Pick markers that read the same as the shape.** If the shape is `circle`, the marker should be a filled disc, not a circled dot. They're not the same renderer, but they should communicate the same idea.
3. **Don't over-collapse.** The temptation is to derive `marker` from `shape` automatically (e.g. `circle → ●`, `ring → ○`). Resist it: the producer often knows exactly which Unicode character renders well in their target font and will pick more precisely than any mapping.

---

## A small Unicode caveat

Worth flagging while we're here: `_first_glyph(...)` in `rich_ui/renderer.py` is essentially:

```python
return value.strip()[0]
```

That's *codepoint*-indexed, not *grapheme*-indexed. A multi-codepoint emoji — anything with a ZWJ sequence, regional indicator, or modifier — gets silently truncated to its first codepoint. For BMP symbols like `●`, `◆`, `▲`, `⊙` this is fine. For `🪐` or `👨‍🚀` it isn't. Not the bug today, but worth knowing the next time someone wants Saturn in a panel.

---

## Code

The change is one character in [`examples/voyager/fetchers/horizons.py`](https://github.com/janmaru/mahamudra-rich-ui/blob/main/examples/voyager/fetchers/horizons.py). The lesson — *one DSL, two renderers, two contracts* — is bigger than that.
