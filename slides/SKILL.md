---
name: slides
description: Create HTML slide presentations from reference materials. Use when users ask to create slides, presentations, decks, or pitch materials. Accepts any input files (docs, PDFs, images, markdown) and produces clean HTML slides with CSS animations, keyboard navigation, and animated charts. Preferred over PowerPoint generation.
---

# Slides

Create professional HTML slide presentations with PPT-like navigation from any reference materials.

## Workflow

### 1. Gather context

Read all provided reference files to understand:
- Main topic and key messages
- Technical details, metrics, outcomes
- Images, diagrams, logos to include

### 2. Draft content in markdown

Create a detailed markdown outline:

```markdown
# Presentation Title

## Slide 1: Title
- Main title
- Subtitle
- Author/date

## Slide 2: Problem
- Pain point 1
- Pain point 2

## Slide 3: Solution
...
```

Ask user to review before proceeding.

### 3. Generate HTML slides

Use the base template from `assets/base-template.html` as starting point. The template includes:
- Keyboard navigation (←/→, Space, Enter)
- Navigation buttons (prev/next)
- Progress bar
- Fullscreen mode (F key)
- Touch/swipe support
- Animated charts and counters

Apply these constraints:

**Critical: No overlapping content**
- Test each slide visually fits within 1280x720
- Use `overflow: hidden` on slide containers
- Limit bullet points to 4-5 per slide
- Keep titles under 60 characters

**Layout selection:**
- `title-slide` - Opening/closing slides
- `content-slide` - Bullet points, paragraphs
- `two-column` - Comparisons, before/after
- `image-left` / `image-right` - Image with text

**Brand customization:**
- Update CSS variables in `:root` for colors
- Place logo in slide footer
- Use dark mode (`.dark` class) by default

### 4. Preview and iterate

Open HTML in browser. Common fixes:
- Text overflow: Reduce content or font size
- Image sizing: Use `object-fit: contain`
- Spacing issues: Adjust padding/margins

## Template Reference

Base template location: `assets/base-template.html`

### CSS Variables
```css
--primary-color: #2563eb;    /* Headings, accents */
--text-color: #1f2937;       /* Body text */
--bg-color: #ffffff;         /* Slide background */
```

### Slide Classes
| Class | Use Case |
|-------|----------|
| `.title-slide` | Title, section breaks |
| `.content-slide` | Standard content |
| `.two-column` | Side-by-side layouts |
| `.dark` | Dark mode theme |

### Animation Classes
| Class | Effect |
|-------|--------|
| `.animate-fade` | Fade in from below |
| `.animate-left` | Slide from left |
| `.animate-right` | Slide from right |
| `.animate-scale` | Scale in |
| `.animate-bounce` | Continuous bounce |
| `.delay-1` to `.delay-8` | Stagger animations |

Animations only play when slide becomes active.

### Keyboard Shortcuts
| Key | Action |
|-----|--------|
| `→` `Space` `Enter` | Next slide |
| `←` `Backspace` | Previous slide |
| `Home` `↑` | First slide |
| `End` `↓` | Last slide |
| `F` | Toggle fullscreen |

## Common Patterns

### Metrics display
```html
<div class="metrics">
    <div class="metric">
        <div class="metric-value">99.9%</div>
        <div class="metric-label">Uptime</div>
    </div>
</div>
```

### Code blocks
```html
<div class="code-block">
    <code>function example() { ... }</code>
</div>
```

### Quote/callout
```html
<div class="quote-box">
    "Important quote or callout text here"
</div>
```

### Bar Chart (animated)
```html
<div class="bar-chart">
    <div class="bar-item">
        <div class="bar-value">85%</div>
        <div class="bar" style="height: 85%;"></div>
        <div class="bar-label">Q1</div>
    </div>
</div>
```

### Animated Counter
```html
<div class="metric-value" data-target="99.9">0</div>
```
Counter animates from 0 to target when slide becomes active.

## Custom Animations

For specialized animations (token streaming, failover diagrams), describe the desired effect and implement with CSS keyframes:

```css
@keyframes tokenStream {
    from { opacity: 0; }
    to { opacity: 1; }
}

.token {
    animation: tokenStream 0.1s ease-out both;
}
.token:nth-child(1) { animation-delay: 0.1s; }
.token:nth-child(2) { animation-delay: 0.2s; }
/* ... */
```

## Advanced Patterns

For complex visualizations (diagrams, timelines, animated demos), see [references/advanced-patterns.md](references/advanced-patterns.md).

## Output

Single self-contained HTML file. All styles inline. Opens in any browser. Print to PDF if needed.
