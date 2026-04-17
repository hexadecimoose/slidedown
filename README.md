# SlideDown — Standalone Slide Deck

A zero-dependency, single-file HTML presentation app. Works offline.
Just open `slideshow.html` in any modern browser.

## Quick Start

1. Open `slideshow.html` in Chrome, Firefox, Safari, or Edge
2. Press **F** for fullscreen
3. Use **→** / **←** to navigate

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| → ↓ Space PgDn | Next slide / fragment |
| ← ↑ PgUp | Previous slide / fragment |
| Home | First slide |
| End | Last slide |
| 1–9 + Enter | Jump to slide number |
| F | Toggle fullscreen |
| S | Open presenter view (new window) |
| O or G | Toggle slide overview grid |
| ? or H | Show keyboard shortcut help |
| Ctrl+P | Print / Export to PDF |

## Touch / Mobile

- **Swipe left** → next slide
- **Swipe right** → previous slide
- **Tap right third** → next slide
- **Tap left third** → previous slide

## Presenter View

Press **S** to open a presenter window showing:
- Current slide
- Next slide preview
- Speaker notes
- Elapsed time clock (with reset button)

Navigation in either window stays synchronized.

## Export to PDF

1. Press **Ctrl+P** (or Cmd+P on Mac)
2. Set orientation to **Landscape**
3. Enable **Background graphics** in print settings
4. Choose **Save as PDF**

All fragments are fully revealed in the PDF. Speaker notes are excluded.

## Editing Slides

Slides are `<section>` elements inside `<div class="sd-deck">`. To add a slide:

```html
<section data-transition="fade">
  <h2>Your Title</h2>
  <ul>
    <li class="fragment">Revealed one at a time</li>
    <li class="fragment">On each key press</li>
  </ul>
  <aside class="notes">Speaker notes go here.</aside>
</section>
```

### Transition Types

Set `data-transition` on any `<section>`:
- `fade` (default) — crossfade
- `slide` — horizontal push
- `none` — instant

### Fragments

Add `class="fragment"` to any element to reveal it incrementally.
Optionally set `data-index="N"` to control reveal order.

### Special Slide Layouts

- `class="title-slide"` — centered, larger text (for title/cover slides)
- `class="impact-slide"` — centered, large heading (for key statements)

### Theming

All colors are CSS custom properties in `:root`. Change the theme by
editing the variables at the top of the `<style>` block:

```css
:root {
  --bg: #0f111a;
  --bg-slide: #1a1d2e;
  --text: #cdd6f4;
  --heading: #ffffff;
  --accent: #89b4fa;
  --accent-alt: #f38ba8;
  /* ... */
}
```

## File Size

The entire app + 10 demo slides: **~50 KB**. No images, fonts, or
external dependencies of any kind.
