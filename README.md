# Presentations

Web-based presentations powered by [Hugo](https://gohugo.io/) and [reveal.js 6.0.1](https://revealjs.com/).

## Quick Start

```bash
# Run locally
hugo server

# Create a new presentation
hugo new content presentations/my-talk/index.md
```

## Writing Slides

Each presentation lives in its own folder under `content/presentations/`:

```
content/presentations/my-talk/
├── index.md       # Slides and front matter
└── style.css      # Optional custom styles (auto-loaded)
```

Wrap each slide in `<section>` tags:

```markdown
---
title: 'My Talk'
date: 2026-06-11
description: 'A talk about something'
event: 'Conference 2026'
tags: ['topic']
theme: white
---

<section>

## Title Slide

Speaker · Event · Year

</section>

<section>

## Another Slide

Content here. Markdown works inside sections.

</section>
```

For **vertical slides** (press down), nest sections:

```html
<section>
<section>

## Main Slide

</section>
<section>

## Sub-slide

</section>
</section>
```

For **speaker notes** (press S), add `Note:` inside a section:

```markdown
<section>

## My Slide

Note:
These notes are only visible in speaker view.

</section>
```

## Per-Presentation Styling

Each presentation can have a completely different look and feel.

### Reveal.js Themes

Set `theme` in front matter:

```yaml
theme: black  # white, black, moon, night, league, beige, sky, serif, solarized
```

### Google Fonts

```yaml
fonts:
  - 'Inter:wght@400;700'
  - 'Fira Code:wght@400'
```

### Custom CSS

Drop a `style.css` file next to `index.md` — it's loaded automatically:

```css
.reveal h2 {
  color: #e94560;
  font-family: 'Inter', sans-serif;
}
```

Or use inline CSS in front matter for quick overrides:

```yaml
customCSS: |
  .reveal { background: linear-gradient(135deg, #1a1a2e, #16213e); }
```

### Reveal.js Options

Pass options directly to `Reveal.initialize()`:

```yaml
revealOptions: |
  transition: 'fade',
  slideNumber: true,
```

## Security

All reveal.js CDN resources include [Subresource Integrity](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity) (SRI) hashes.

## Build & Deploy

```bash
hugo              # Build to public/
hugo --minify     # Build minified
```

The `public/` directory can be deployed to any static hosting (GitHub Pages, Netlify, Cloudflare Pages, etc.).
