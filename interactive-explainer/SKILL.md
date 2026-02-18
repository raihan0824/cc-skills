---
name: interactive-explainer
description: Create interactive, step-by-step HTML educational visualizations with GitHub-dark theme, navigation controls, and animated state-driven rendering. Use when the user asks to create an interactive explainer, educational visualization, step-through animation, technical concept walkthrough, or any interactive HTML page that teaches a concept through sequential steps. Triggers on phrases like "create an interactive explainer", "make a visualization for", "build a step-by-step animation", "interactive HTML page about", "teach this concept visually", or "create an educational walkthrough".
---

# Interactive Explainer

Create self-contained, single-file HTML educational visualizations. Each explainer walks through a technical concept step-by-step with controls, narration, and animated visuals using a GitHub-dark theme.

## Workflow

1. Understand the concept and break it into 5-8 sequential steps
2. Copy the template from `assets/template.html` as the starting point
3. Design the visualization (grids, timelines, diagrams, etc.)
4. Define step data arrays and narrations
5. Implement the `render()` function as a pure state machine
6. Add metrics bar and explanation section
7. Verify footer is the LAST element inside `.container`

## Template

Start every explainer by copying `assets/template.html`. It contains the full boilerplate: theme CSS, controls, progress bar, narration box, and the JavaScript state machine (go/togglePlay/playTick/updatePlayBtn/render).

Replace `{{placeholders}}` with actual content. Add custom CSS where marked `{{CUSTOM_CSS}}`.

## Design System

See [references/design-system.md](references/design-system.md) for the complete color palette, typography, component specs, SVG icons, and common visualization elements (request blocks, state badges, utilization bars, legends).

## Architecture Rules

### Pure state machine
The `render()` function MUST rebuild ALL visual state from `currentStep` alone. No incremental mutations. This ensures Prev/Next work correctly in both directions.

### Data-driven steps
Define step states as arrays indexed by step number:
```javascript
var stepData = [
  { items: [], util: 0 },        // Step 0: empty
  { items: ['A'], util: 25 },    // Step 1
  { items: ['A','B'], util: 50 } // Step 2
];
function render() {
  var d = stepData[currentStep];
  // rebuild DOM from d
}
```

### Narrations array
```javascript
var narrations = [
  ['Getting Started', 'Click <span class="hl">Next</span> to step through...'],
  ['Step 1 — Title', 'Explanation with <span class="green">highlights</span>.'],
];
```
Step 0 is always the "Getting Started" intro. Use `.hl` (blue), `.green`, `.red`, `.yellow` for inline highlights.

### Tabbed variant
For multi-concept explainers, use tabs with indexed IDs (`prevBtn0`, `prevBtn1`, `scene0`, `scene1`). Each tab gets its own state, controls, and render function. See `kv-cache-vs-prefix-cache.html` as reference.

## Layout Order

Elements inside `.container` MUST follow this order:
1. `h1` + `.subtitle`
2. `.controls`
3. `.progress-bar`
4. `.narration`
5. Visualization content (custom)
6. `.metrics-bar` (optional)
7. `.explanation`
8. `.footer` — ALWAYS last element inside `.container`

## Checklist

Before delivering, verify:
- [ ] Single self-contained HTML file (no external dependencies)
- [ ] GitHub-dark theme colors only (see design-system.md)
- [ ] Controls at top: Prev / Next / Play-Stop / Speed / Step label
- [ ] Progress bar with done/current dot states
- [ ] Narration box updates per step
- [ ] `render()` is a pure function of `currentStep`
- [ ] Prev/Next/Play/Stop all work correctly
- [ ] Footer is the LAST element inside `.container`
- [ ] Footer reads: `Created by <a href="https://github.com/raihan0824" target="_blank">Raihan Afiandi</a>`
