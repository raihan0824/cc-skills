---
name: drawio-architect
description: Generate elegant, professional draw.io (.drawio) XML diagrams from ASCII art, textual descriptions, or architectural specifications. Use when the user asks to create a drawio diagram, make an architecture diagram, convert ASCII art to drawio, create a flowchart, ER diagram, sequence diagram, network topology, or any technical visualization as a .drawio file. Triggers on phrases like "create a drawio", "make a diagram", "draw this architecture", "convert this ASCII to drawio", "make this elegant drawio", "visualize this as drawio", "create a flowchart", "ER diagram", "sequence diagram".
---

# Draw.io Architect

Generate professional `.drawio` XML files from any input: ASCII art, textual descriptions, bullet-point specs, or existing diagrams that need restyling.

## Workflow

1. **Analyze input** — Identify diagram type (architecture, flow, sequence, ER, network, hierarchy, comparison)
2. **Select theme** — Default to "Corporate Dark" unless user specifies. Read [references/themes.md](references/themes.md) for available themes and color palettes
3. **Select pattern** — Read [references/diagram-patterns.md](references/diagram-patterns.md) for the matching diagram type's layout structure
4. **Build XML** — Use element templates from [references/drawio-elements.md](references/drawio-elements.md) to construct the `.drawio` file
5. **Write file** — Save as `.drawio` to the user's specified or contextually appropriate path

## Design Principles

- **Sections with labels**: Group related elements under uppercase section labels with divider lines
- **Colored card system**: Each semantic group gets a distinct color from the theme palette
- **Card headers**: Major elements get colored header bars with icon + title
- **Inner detail rows**: Properties shown as labeled white inner boxes within cards
- **Pills/tags**: Small categorized items (skills, features, tags) as rounded pill badges
- **Arrows**: Solid for sequence/flow, dashed for communication/dependency, colored to match target
- **Legend bar**: Always include a bottom legend explaining colors and arrow types
- **Icons**: Use Unicode emoji for visual anchoring (see drawio-elements.md for icon table)
- **Shadows**: Enable `shadow=1` on outer frame and cards (except Monochrome theme)
- **Grid alignment**: Snap all positions to multiples of 10

## Sizing Defaults

| Element | Width | Height |
|---------|-------|--------|
| Page (medium) | 1100 | 760 |
| Page (large) | 1200 | 900 |
| Title bar | full width | 50-55 |
| Card (standard) | 280-340 | varies |
| Card header | match card | 45-50 |
| Inner row | match card - 20px padding | 38-42 |
| Pill/tag | 80-130 | 28-30 |
| Badge | 50-120 | 24-28 |
| Legend | full width - 60px margin | 50-55 |
| Gap between cards | 20-30 | — |
| Padding inside cards | 20 | 10-15 |

## Theme Selection

If user specifies a style preference, map it:
- "dark" / "neon" / "vibrant" → Neon
- "minimal" / "clean" / "print" → Monochrome
- "warm" / "earthy" → Warm Sunset
- "blue" / "cool" / "ocean" → Ocean Breeze
- "professional" / "corporate" / default → Corporate Dark

Read [references/themes.md](references/themes.md) for full color tables.

## References

- [references/themes.md](references/themes.md) — 5 theme palettes with full color tables
- [references/diagram-patterns.md](references/diagram-patterns.md) — Layout patterns for 7 diagram types
- [references/drawio-elements.md](references/drawio-elements.md) — XML templates for all elements, arrows, text, icons
