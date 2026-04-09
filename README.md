# Content to HTML

Turn long-form articles into slides, pages, decks, and posters. A Claude Code skill with 6 output formats, 15 slide types, 8 style presets, and a JSON-driven rendering engine.

## What It Does

| Format | Canvas | Input | Output |
|--------|--------|-------|--------|
| **page** | Responsive scroll | One article | Readable HTML page |
| **slides** | 1280x720 (16:9) | One article | Key-point presentation |
| **deck** | 1280x720 (16:9) | Multiple sources | Curated narrative presentation |
| **poster-9x16** | 1080x1920 | Article highlights | Vertical poster |
| **poster-1x1** | 1080x1080 | Core insight | Square social card |
| **poster-16x9** | 1920x1080 | Article summary | Horizontal share card |

## Architecture

```
SKILL.md                  ← Entry point: phase flow + commands
  |
  ├── format-specs.md     ← Extraction pipeline (6 steps) + per-format specs
  ├── slide-types.md      ← 15 slide types with px-perfect layout
  ├── slide-design-principles.md  ← Design theory (Rule of Thirds, whitespace, etc.)
  |
  ├── STYLE_PRESETS.md    ← 8 visual presets (CSS vars + fonts + atmosphere)
  ├── html-template.md    ← HTML skeleton for slides/deck/poster
  ├── viewport-base.css   ← Shared CSS (fixed card + scroll page modes)
  ├── animation-patterns.md  ← Animation-by-role system
  ├── mermaid-guide.md    ← Mermaid integration + 7 gotchas
  |
  ├── slide-schema.md     ← JSON contract for 15 slide types
  ├── slide-engine.html   ← Self-contained renderer (JSON in → HTML out)
  |
  └── quality-gate.md     ← 3-layer defense (pre/during/post generation)
```

### Two Generation Paths (v4.0)

| Path | Sub-agent outputs | Design rules |
|------|-------------------|-------------|
| **JSON→Engine** (recommended) | JSON per slide-schema.md | Locked in slide-engine.html |
| **Direct HTML** | Complete HTML+CSS | In slide-types.md specs |

## Key Design Decisions

- **Assertion-Evidence method**: Slide titles are conclusion sentences, not topic labels
- **Density coefficient D** (1.0 / 1.25 / 1.6): Multiplies all spacing; font sizes never change
- **1280x720 fixed canvas**: Pixel-perfect control, no responsive scaling
- **Extraction Plan Table**: User reviews and approves content mapping before generation
- **Card Anti-Float**: `color-mix(in srgb, var(--bg) 92%, var(--text) 8%)` prevents floating cards

## Usage

```
/content-to-html slides path/to/article.md
/content-to-html page path/to/article.md
/content-to-html deck
/content-to-html poster path/to/article.md
```

## Version

v4.0 (2026-04-09) — JSON→Engine architecture, density coefficient, 15 slide types, 8 presets.

## Credits

Page design: [ship-page-skill](https://github.com/Zooeyii/ship-page-skill), [visual-explainer](https://github.com/nicobailon/visual-explainer)
Slides layout: [makino-data-slides](https://github.com/makinotes/makino-data-slides), [revealjs-skill](https://github.com/ryanbbrown/revealjs-skill)
Style system: [zara-frontend-slides](https://github.com/anthropics/zara-frontend-slides) (MIT)
