# Design System Reference

## Color Palette (GitHub-Dark)

| Token | Hex | Usage |
|---|---|---|
| `bg-primary` | `#0d1117` | Page background, inner containers |
| `bg-secondary` | `#161b22` | Panels, cards, narration, metrics, explanation |
| `bg-tertiary` | `#21262d` | Default buttons, empty states |
| `border-default` | `#30363d` | All borders |
| `border-muted` | `#21262d` | Subtle/empty borders |
| `accent-blue` | `#58a6ff` | Headings, highlights, accent text |
| `accent-green` | `#3fb950` | Success, positive values |
| `accent-green-bg` | `#238636` | Primary button bg |
| `accent-green-hover` | `#2ea043` | Primary button hover & border |
| `accent-red` | `#f85149` | Danger, negative values |
| `accent-yellow` | `#e3b341` | Warning, cached/reused |
| `accent-purple` | `#a371f7` | Generated, special |
| `text-primary` | `#e6edf3` | Main body text |
| `text-secondary` | `#c9d1d9` | Narration body, description text |
| `text-muted` | `#8b949e` | Labels, subtitles |
| `text-faint` | `#484f58` | Footer text, disabled |

## Typography

- Font stack: `'Segoe UI', system-ui, -apple-system, sans-serif`
- h1: `1.5rem`, color `#58a6ff`, center-aligned
- Subtitle: `0.85rem`, color `#8b949e`, center-aligned
- Labels/titles: `0.75rem-0.85rem`, `uppercase`, `letter-spacing: 0.04em`, `font-weight: 700`
- Body text: `0.85rem-0.88rem`, `line-height: 1.6`
- Metric values: `1.2rem`, `font-weight: 800`

## Component Patterns

### Controls Bar
- Flex row with `gap: 8px`, `flex-wrap: wrap`
- Order: Prev | Next | Play/Stop | Speed selector | Step label (right-aligned)
- Prev button: `.btn` (default gray)
- Next button: `.btn.primary` (green)
- Play/Stop toggle: `.btn.play` (blue) / `.btn.stop` (red) with SVG icon swap
- Speed select: Slow (3000ms) / Normal (1800ms) / Fast (1000ms)

### SVG Icons
Prev chevron:
```html
<svg viewBox="0 0 16 16"><path d="M9.78 12.78a.75.75 0 01-1.06 0L4.47 8.53a.75.75 0 010-1.06l4.25-4.25a.75.75 0 011.06 1.06L6.06 8l3.72 3.72a.75.75 0 010 1.06z"/></svg>
```
Next chevron:
```html
<svg viewBox="0 0 16 16"><path d="M6.22 3.22a.75.75 0 011.06 0l4.25 4.25a.75.75 0 010 1.06l-4.25 4.25a.75.75 0 01-1.06-1.06L9.94 8 6.22 4.28a.75.75 0 010-1.06z"/></svg>
```
Play triangle:
```html
<svg viewBox="0 0 16 16"><path d="M4 2l10 6-10 6z"/></svg>
```
Stop square:
```html
<svg viewBox="0 0 16 16"><rect x="3" y="3" width="10" height="10" rx="1"/></svg>
```

### Progress Bar
- Flex row of dots, `gap: 3px`, each dot `flex: 1`, `height: 4px`
- States: default (`#21262d`), `.done` (`#238636`), `.current` (`#58a6ff`)

### Narration Box
- `background: #161b22`, `border: 1px solid #30363d`, `border-radius: 8px`
- `.step-title`: uppercase, `0.75rem`, `#58a6ff`
- `.step-body`: `0.88rem`, `#c9d1d9`
- Inline highlight classes: `.hl` (blue), `.green`, `.red`, `.yellow`

### Metrics Bar
- Flex row, `gap: 12px`, each metric card `flex: 1`, `min-width: 120px`
- Label: uppercase `0.65rem`, muted
- Value: `1.2rem`, `font-weight: 800`, colored by `.red`/`.green`/`.blue`

### Explanation Section
- Panel with `padding: 20px`, `margin-bottom: 16px`
- h3: uppercase `0.85rem`, `#58a6ff`
- Paragraphs: `0.85rem`, `line-height: 1.6`

### Footer
- ALWAYS the last element inside `.container`
- `text-align: center`, `margin-top: 20px`, `padding: 12px`
- Text: `0.8rem`, `#484f58`
- Link: `#58a6ff`, no underline, underline on hover
- Content: `Created by <a href="https://github.com/raihan0824" target="_blank">Raihan Afiandi</a>`

## Layout Order (top to bottom)

1. `h1` title
2. `.subtitle`
3. `.controls` bar
4. `.progress-bar`
5. `.narration` box
6. Visualization content (custom per topic)
7. `.metrics-bar` (optional)
8. `.explanation` section
9. `.footer` (always last inside `.container`)

## JavaScript Architecture

### State Machine Pattern
- `currentStep` (int, starts at 0)
- `MAX` (total number of steps)
- `playing` (boolean), `playTimer` (timeout ref)
- Pure `render()` function: rebuilds ALL visual state from `currentStep` alone — no incremental mutations
- `narrations` array: `[['Step Title', 'Body HTML with <span class="hl">highlights</span>'], ...]`
- Step 0 is always "Getting Started" intro

### Data-Driven Rendering
Define step data as arrays indexed by step number. The `render()` function reads the array at `currentStep` and rebuilds the DOM. This avoids fragile incremental state.

Example pattern:
```javascript
var stepData = [
  // Step 0: initial empty state
  { items: [], utilization: 0 },
  // Step 1: first change
  { items: ['A'], utilization: 25 },
  // ...
];

function render() {
  var data = stepData[currentStep];
  // Rebuild DOM from data
}
```

### Tabbed Variant
For multi-concept explainers (e.g., KV Cache vs Prefix Cache):
- Tabs at top with `.tab` / `.tab.active`
- Separate `.scene` divs (`.hidden` for inactive)
- Each tab has its own controls, state, and render function with indexed IDs (e.g., `prevBtn0`, `prevBtn1`)
- `switchTab(idx)` toggles `.active` and `.hidden`

## Common Visualization Elements

### Color-coded request blocks
```css
.req-1 { background: #1a0a2e; border-color: #8957e5; color: #a371f7; } /* purple */
.req-2 { background: #122d1e; border-color: #2ea043; color: #3fb950; } /* green */
.req-3 { background: #0c2d48; border-color: #388bfd; color: #58a6ff; } /* blue */
.req-4 { background: #2a1d00; border-color: #d29922; color: #e3b341; } /* yellow */
.req-5 { background: #2d0a1e; border-color: #da3633; color: #f85149; } /* red */
```

### State badges
- Empty: `background: #161b22; border-color: #21262d; color: #484f58`
- Computing/Active: `background: #1a1e2e; border-color: #58a6ff; color: #58a6ff` + pulse animation
- Completed: `background: #122d1e; border-color: #2ea043; color: #3fb950`
- Cached/Reused: `background: #2a1d00; border-color: #d29922; color: #e3b341`
- Wasted: `background: #2a1d00; border-color: #9e6a03; color: #e3b341; opacity: 0.7`

### Pulse animation
```css
@keyframes pulse { 0%,100% { opacity: 1; } 50% { opacity: 0.5; } }
.computing { animation: pulse 0.8s ease-in-out infinite; }
```

### GPU utilization bars
```css
.util-bar { height: 16px; background: #0d1117; border-radius: 8px; border: 1px solid #21262d; }
.util-fill { height: 100%; border-radius: 8px; transition: width 0.4s ease; }
```

### Legend
- Flex row, `gap: 14px-16px`
- Each item: dot (14x14px, `border-radius: 3px`) + label (`0.72rem-0.75rem`, muted)
