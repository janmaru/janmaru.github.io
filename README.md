# Janmaru — Thoughts & Architectures

Personal blog by **Mauro Ghiani** (janmaru), hosted on GitHub Pages with Jekyll.

**Live:** [janmaru.github.io](https://janmaru.github.io)

## Quick Start

```bash
# Local development
gem install bundler jekyll
bundle install
bundle exec jekyll serve --future
```

The site is live at `http://localhost:4000`.

## Project Structure

```
janmaru.github.io/
├── _config.yml          # Jekyll configuration
├── _layouts/
│   ├── default.html     # Base HTML shell (head, header, nav, footer)
│   ├── home.html        # Homepage with paginated post list
│   ├── post.html        # Individual article layout
│   └── category.html    # Category archive page
├── _posts/              # Markdown articles (YYYY-MM-DD-slug.md)
├── categories/          # Category index pages (e.g., llm.md)
├── articles/            # Generated article URLs land here (via permalink)
├── css/
│   └── style.css        # Full site styling (retro IDE theme)
├── about.md             # Author bio and selected projects
├── index.html           # Homepage entry point (uses home layout)
├── docs/                # Technical and functional documentation
│   ├── technical.md     # Architecture, config, layout system details
│   └── functional.md    # Content strategy, design decisions, roadmap
└── errors/              # Screenshots for debugging (gitignored)
```

## Key Configuration

| Setting | Value | Purpose |
|---|---|---|
| `permalink` | `/articles/:slug/` | Clean English URLs for posts |
| `future` | `true` | Render posts with future dates |
| `theme` | `null` | No gem theme, fully custom layouts |
| `paginate` | `5` | Posts per page on homepage |
| `paginate_path` | `/page/:num/` | Pagination URL pattern |

## Design

Retro IDE / Turbo Pascal aesthetic with modern readability:
- Dark background (`#1e1e2e`), monospace fonts throughout
- Cyan titles, green accents, yellow hover states
- Comfortable contrast and `1.8` line-height for long reads
- Responsive layout for mobile

## Writing a New Post

Create a file in `_posts/` following this pattern:

```markdown
---
title: "Your Post Title"
date: 2026-04-15
author: Mauro Ghiani
category: LLM
tags: [Tag1, Tag2]
excerpt: "One-line summary for the homepage card."
---

Your content in Markdown here.
```

## Adding a New Category

1. Create `categories/your-category.md`:
   ```markdown
   ---
   title: "Category: YourCategory"
   layout: category
   permalink: /categories/your-category/
   category_name: YourCategory
   category_description: Description of the category.
   ---
   ```
2. Add a nav link in `_layouts/default.html`

## Multiple Blogs

Additional blogs can be created as separate repos. Each gets served at `janmaru.github.io/<repo-name>/`. Set `baseurl: /<repo-name>` in that repo's `_config.yml`.

## Documentation

- [Technical Analysis](docs/technical.md) — Architecture, layout system, build pipeline
- [Functional Analysis](docs/functional.md) — Content model, design rationale, roadmap
