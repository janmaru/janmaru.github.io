# Functional Analysis

## Purpose

A personal technical blog exploring intersections between mathematics, philosophy, and software architecture — with a current focus on Large Language Models (LLMs).

## Author Profile

- **Name:** Mauro Ghiani (janmaru)
- **Role:** Senior Developer
- **Location:** Taipei, Taiwan
- **Stack:** C#, Scala, F#
- **Interests:** Functional programming, zero-knowledge proofs, category theory, meta-rationality

## Content Model

### Posts
- Long-form technical articles written in Markdown
- Each post belongs to exactly one **category** and can have multiple **tags**
- Writing style: conceptual narratives that connect abstract ideas (myths, math, probability theory) to concrete engineering topics (KV cache, context windows, prompt engineering)
- Bilingual author — content is in English

### Categories
- Currently one category: **LLM**
- Each category has a dedicated archive page at `/categories/<name>/`
- New categories require a Markdown file in `categories/` and a nav link in the default layout

### Tags
- Freeform, defined per-post in frontmatter
- Displayed on individual post pages as `#tag` badges
- Not currently linked to tag archive pages (potential future feature)

## Current Content

| Post | Date | Category | Theme |
|---|---|---|---|
| How Is Algebra Related to AI's Context Windows... | 2026-03-27 | LLM | Connects Enuma Elish myth → JL Lemma → Google TurboQuant KV cache compression |
| Beyond Conventions: Why LLM Context Management Requires Explicit Configuration | 2026-04-01 | LLM | Probability theory applied to LLM context management, argues configuration over convention |

## Design Rationale

### Retro IDE Theme
Inspired by the Turbo Pascal DOS environment — dark background, monospace fonts, cyan/green color palette. Chosen to reflect the author's deep programming roots while maintaining modern readability standards:
- Line-height `1.8` for comfortable reading
- WCAG-conscious contrast ratios (light text on dark, not pure white on pure black)
- Responsive layout for mobile devices

### Minimalism
- No JavaScript (pure HTML/CSS)
- No analytics, no cookies, no external dependencies
- Single CSS file, four layouts — nothing more than what's needed

## Navigation Structure

```
Home (/)
├── Post list (paginated, 5 per page)
├── AI (/categories/ai/)
│   └── Filtered post list for AI category
├── Philosophy (/categories/philosophy/)
│   └── Filtered post list for Philosophy category
├── Odds (/categories/odds/)
│   └── Filtered post list for Odds category
└── About (/about/)
    └── Bio, interests, selected projects
```

## Scalability Considerations

### Pagination
- Enabled via `jekyll-paginate` (5 posts per page)
- Pagination nav auto-appears when posts exceed the per-page limit
- Supported natively on GitHub Pages

### Search
- No search currently implemented
- Future option: client-side search with **lunr.js** (builds index at build time, searches in browser)
- Viable up to a few hundred posts without performance issues

### Multiple Blogs
- Additional topic-focused blogs can live in separate repos
- Served at `janmaru.github.io/<repo-name>/`
- Each is an independent Jekyll site with its own config, layouts, and content

### Future Enhancements (Potential)
- Tag archive pages (`/tags/<tag>/`)
- Reading time estimate per post
- Client-side search (lunr.js)
- RSS feed (`jekyll-feed` plugin, supported on GitHub Pages)
- Dark/light theme toggle
