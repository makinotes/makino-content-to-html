# Content to HTML

Turn long-form articles into slides, pages, decks, and posters. A Claude Code skill with 6 output formats, 15 slide types, 8 style presets, and a JSON-driven rendering engine.

## Why this exists

Claude Code can write HTML directly — so why a skill?

Because consistent, design-grade output is hard to get from "make me a slide" prompts. You end up re-tweaking the same things every time: spacing, font hierarchy, slide density, color systems, when to break to a new slide. This skill encodes those decisions — extraction pipeline, slide types, design rules, style presets — so you get a usable result on the first generation.

For Claude Code users who want to turn a long-form article (markdown / draft / notes) into something shareable: a readable web page, a slide deck, or a poster card — without re-prompting layout every time.

## What It Does

| Format | Canvas | Input | Output |
|--------|--------|-------|--------|
| **page** | Responsive scroll | One article | Readable HTML page |
| **slides** | 1280x720 (16:9) | One article | Key-point presentation |
| **deck** | 1280x720 (16:9) | Multiple sources | Curated narrative presentation |
| **poster-9x16** | 1080x1920 | Article highlights | Vertical poster |
| **poster-1x1** | 1080x1080 | Core insight | Square social card |
| **poster-16x9** | 1920x1080 | Article summary | Horizontal share card |

### When to use which

| Goal | Format |
|------|--------|
| Long article to read on the web | `page` |
| Distill into a 10-15 slide presentation | `slides` |
| Combine multiple articles into a curated narrative | `deck` |
| Vertical card for social (Xiaohongshu / IG story) | `poster-9x16` |
| Square card (Twitter / WeChat moments) | `poster-1x1` |
| Horizontal banner (LinkedIn / blog header) | `poster-16x9` |

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

## Install

```bash
cd ~/.claude/skills/
git clone https://github.com/makinotes/makino-content-to-html.git content-to-html
```

Then type `/content-to-html` in Claude Code.

## Usage

```
/content-to-html slides path/to/article.md
/content-to-html page path/to/article.md
/content-to-html deck
/content-to-html poster path/to/article.md
```

## Version

v4.2 (2026-04-19) — Style preset calibration (Apple/Stripe/Linear/Vercel/Material refs), body font mono→sans, desaturated accents. Earlier: v4.1 slide interactions (click-to-reveal, number rolling), v4.0 JSON→Engine architecture + density coefficient + 15 slide types + 8 presets.

## Known limitations

- 1280x720 fixed canvas — pixel-perfect but not responsive. Mobile-friendly slides aren't a goal.
- Best on long-form articles (1500+ chars). Shorter pieces feel padded.
- Style presets are opinionated. Custom brand identity needs editing STYLE_PRESETS.md.
- Mermaid integration has known gotchas — see mermaid-guide.md.
- Generation quality depends on the model — works best with Sonnet/Opus, less reliable on Haiku.

## Roadmap

Possible directions if there's interest:

- Web playground for non-Claude-Code users
- Style preset editor (currently markdown-edit only)
- Better Chinese typography defaults
- Export to PDF / PPTX from the rendered HTML

## Contributing

I'm not actively maintaining this — it solved a problem for me and now it's out there. If something is broken or you want a feature, PRs are the fastest path forward. Issues are fine but I may not respond quickly.

## Credits

Page design: [ship-page-skill](https://github.com/Zooeyii/ship-page-skill), [visual-explainer](https://github.com/nicobailon/visual-explainer)
Slides layout: [makino-data-slides](https://github.com/makinotes/makino-data-slides), [revealjs-skill](https://github.com/ryanbbrown/revealjs-skill)
Style system: zara-frontend-slides by zarazhangrui (MIT)

## License

MIT — see [LICENSE](LICENSE).
