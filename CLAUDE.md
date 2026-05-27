# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A [Marp](https://marp.app/) presentation on LLM fundamentals ("AI Demystified"), authored in Markdown and deployed to GitHub Pages.

## Build

There is no package.json or Makefile. The slide deck is compiled with the Marp CLI:

```bash
marp src/slides.md --output public/index.html
```

In CI, this runs inside the `marpteam/marp-cli:latest` Docker container. Pushing to `main` triggers `.github/workflows/marp.yaml`, which builds and deploys to GitHub Pages automatically.

For local preview, the [VS Code Marp extension](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode) renders slides in the editor — `.vscode/settings.json` already points it at the custom theme.

## Architecture

| Path | Purpose |
|---|---|
| `src/slides.md` | All slide content (single source of truth) |
| `src/marp-theme.css` | Custom dark theme (GitHub Copilot palette: `#0d1117` background, `#58a6ff`/`#bc8cff` accents) |
| `.github/workflows/marp.yaml` | Build + GitHub Pages deploy |
| `public/index.html` | Build artifact (generated, not committed) |

### Marp slide structure

Slides are separated by `---`. The theme is declared in the frontmatter:

```markdown
---
marp: true
theme: custom-theme
---
```

Custom slide layout classes defined in `marp-theme.css`: `lead`, `chapter`, `invert`, `highlight`. Apply them with `<!-- _class: chapter -->` directives inside a slide.
