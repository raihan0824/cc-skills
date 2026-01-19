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
- Add logo.png in same folder or use absolute path
- Customize footer with presentation name

### 4. Preview and iterate

Open HTML in browser. Common fixes:
- Text overflow: Reduce content or font size
- Image sizing: Use `object-fit: contain`
- Spacing issues: Adjust padding/margins

## Template Reference

Base template location: `assets/base-template.html`

### Design System

The template uses a dark, modern aesthetic by default:
- Dark gradient background (`#0a0a0a` → `#1a1a2e`)
- Cyan/teal accent color (`#00d4aa`)
- Glassmorphism cards with subtle borders
- Segoe UI font family

### CSS Variables
```css
--primary-color: #00d4aa;     /* Cyan accent - titles, highlights */
--secondary-color: #00a8cc;   /* Secondary accent */
--accent-color: #667eea;      /* Purple accent */
--text-color: #ffffff;        /* White text */
--text-muted: #888;           /* Muted gray text */
--bg-color: #0a0a0a;          /* Nearly black */
--bg-secondary: #1a1a2e;      /* Dark blue-gray */
--card-bg: rgba(255,255,255,0.05);  /* Glass card */
```

### Slide Classes
| Class | Use Case |
|-------|----------|
| `.title-slide` | Opening/closing with centered content |
| `.section-slide` | Section dividers (01, 02...) |
| `.two-column` | Side-by-side layouts |
| `.three-column` | Card grids |
| No class needed | Standard content slide |

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
    <code><span class="code-keyword">function</span> <span class="code-command">greet</span>(name) {
    <span class="code-keyword">return</span> <span class="code-string">"Hello"</span>;
}</code>
</div>
```
Syntax classes: `.code-keyword`, `.code-string`, `.code-comment`, `.code-command`, `.code-url`

### Highlight box
```html
<div class="highlight-box">
    <strong>Key insight:</strong> Important information here
</div>
```

### Tags (categories)
```html
<span class="tag tag-fix">FIX</span>
<span class="tag tag-improve">IMPROVEMENT</span>
<span class="tag tag-new">NEW</span>
```

### Metrics (animated counter)
```html
<div class="metric-row">
    <div class="metric">
        <div class="metric-value" data-target="99.9">0</div>
        <div class="metric-label">% Uptime</div>
    </div>
</div>
```
Counter animates from 0 to target when slide becomes active.

### Comparison (before/after)
```html
<div class="before-after">
    <div class="before-box">
        <div>😰</div>
        <div>Before</div>
    </div>
    <div class="arrow-icon">→</div>
    <div class="after-box">
        <div>😎</div>
        <div>After</div>
    </div>
</div>
```

### Diagram boxes
```html
<div class="diagram">
    <div class="diagram-box primary">
        <div class="diagram-box-title">Component</div>
        <div class="diagram-box-content">Details</div>
    </div>
    <div class="diagram-arrow"></div>
    <div class="diagram-box secondary">Next</div>
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
