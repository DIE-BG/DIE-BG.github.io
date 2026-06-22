# Repository Conventions

## Site Overview

This repository is a Quarto website for DIE-BG. Source content lives in `.qmd` files and the generated output is published to GitHub Pages through the `gh-pages` target.

Do not edit generated output directories directly:

- `_site/`
- `.quarto/`
- `tmp/`

Navigation is configured in `_quarto.yml`. A page can exist in the repository without being visible on the site; add it to the appropriate navbar or sidebar section when it should be reachable.

## HEFM Navigation

HEFM uses the sidebar with `id: hefm` in `_quarto.yml`. Keep related pages grouped by purpose:

- `Results` for model result pages and SVAR comparisons.
- `Resources` for articles, notes, and reusable material.
- `Documentation` for explanatory or implementation documentation.

Prefer linking to real index pages and final documents instead of placeholder pages when both exist.

## Adding Presentations

Presentations should be authored as Quarto `revealjs` documents. Place each deck near the result or resource it belongs to, usually inside the same model or topic folder.

Use this baseline YAML for decks that should be usable from the embedded site view:

```yaml
---
title: "Presentation Title"
author: "DIE"
format:
  revealjs:
    scrollable: true
    incremental: true
    slide-number: true
    smaller: true
    progress: true
    controls: true
    controls-back-arrows: faded
---
```

The important controls are:

- `controls: true` to show navigation controls.
- `controls-back-arrows: faded` to keep backward navigation available without making it visually dominant.
- `progress: true` and `slide-number: true` to help readers understand their position in the deck.
- `scrollable: true` and `smaller: true` to make dense technical slides fit better in an iframe.

## Embedding Presentations In Index Pages

Each embedded presentation should have an `index.qmd` page that provides:

- A heading for the exercise or topic.
- A fullscreen link to the source `.qmd` deck.
- An HTML iframe that loads the rendered `.html` deck.

Use the shared iframe dimensions from `_variables.yml` unless a page has a specific reason to override them:

````markdown
---
execute:
  freeze: false
---

# Presentation Title

[**Fullscreen**](/path/to/deck.qmd)

```{=html}
<iframe
    src="/path/to/deck.html"
    height={{< var frame_height >}}
    width={{< var frame_width >}}
    style="border: none;"
></iframe>
```
````

Keep iframe paths absolute from the site root, matching the current convention, for example `/hefm/results/.../deck.html`.

## Adding A New HEFM Result Deck

When adding a new result presentation:

1. Create the deck as a `revealjs` `.qmd` file in the relevant result folder.
2. Create or update the local `index.qmd` that embeds the rendered deck with an iframe.
3. Add the `index.qmd` page to the HEFM sidebar in `_quarto.yml`.
4. Keep images in a local `images/` or `plots/` folder next to the content that uses them.

## Publishing

The repository supports publication through GitHub Actions and manual Quarto publishing:

- Pushes to `main` trigger `.github/workflows/publish.yml`.
- Manual publication can be done with `quarto publish gh-pages`.

Before publishing, render or preview locally when possible and verify that embedded decks resolve to their rendered `.html` files.
