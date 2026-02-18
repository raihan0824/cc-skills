# Diagram Patterns

## Table of Contents
- [Architecture Diagram](#architecture-diagram)
- [Flow / Pipeline Diagram](#flow--pipeline-diagram)
- [Sequence Diagram](#sequence-diagram)
- [Entity-Relationship Diagram](#entity-relationship-diagram)
- [Network / Topology Diagram](#network--topology-diagram)
- [Comparison / Matrix Diagram](#comparison--matrix-diagram)
- [Hierarchy / Org Chart](#hierarchy--org-chart)

---

## Architecture Diagram

**Use for**: System architecture, pod architecture, microservices, cloud infrastructure.

**Layout pattern**: Outer container → sectioned rows → cards within each section.

```
Structure:
┌─ Outer Frame (title bar at top) ──────────────┐
│  SECTION LABEL ─────────── divider line        │
│  ┌─ Card ─┐  ┌─ Card ─┐  ┌─ Card ─┐          │
│  │ header │  │ header │  │ header │          │
│  │ body   │  │ body   │  │ body   │          │
│  └────────┘  └────────┘  └────────┘          │
│                                                │
│  SECTION LABEL ─────────── divider line        │
│  ┌─ Wide Card ──────────────────────┐          │
│  └──────────────────────────────────┘          │
│                                                │
│  ┌─ Legend ─────────────────────────┐          │
│  └──────────────────────────────────┘          │
└────────────────────────────────────────────────┘
```

**Key elements**:
- Title bar: full-width colored bar at top of outer frame
- Section labels: uppercase, small font, muted color, with divider line
- Cards: colored header bar + body with labeled rows (LABEL → content box)
- Arrows: dashed between related containers, solid for sequence
- Legend: bottom bar explaining colors/arrows

**Sizing guidelines**:
- Outer frame: 1000-1120px wide, height as needed
- Title bar: 50-55px tall
- Cards: 280-340px wide, 100-350px tall depending on content
- Card headers: 45-50px tall
- Inner rows: 38-42px tall
- Spacing: 20-30px between cards, 20px padding inside cards

## Flow / Pipeline Diagram

**Use for**: CI/CD pipelines, data flows, ETL, request lifecycle.

**Layout pattern**: Horizontal or vertical chain of stages connected by arrows.

```
Structure:
┌─ Outer Frame ─────────────────────────────────┐
│  ┌─ Stage ─┐    ┌─ Stage ─┐    ┌─ Stage ─┐   │
│  │  step   │ ──▶│  step   │ ──▶│  step   │   │
│  │  detail │    │  detail │    │  detail │   │
│  └─────────┘    └─────────┘    └─────────┘   │
│       │                              │         │
│       ▼ (on failure)                 ▼         │
│  ┌─ Branch ─┐              ┌─ Branch ─┐       │
│  └──────────┘              └──────────┘       │
└────────────────────────────────────────────────┘
```

**Key elements**:
- Stages as rounded cards with step number badges
- Solid thick arrows (`strokeWidth=2.5`) between stages
- Branch arrows use dashed style
- Labels on arrows for conditions (e.g., "on failure", "if approved")
- Optional swimlanes as horizontal/vertical bands

## Sequence Diagram

**Use for**: API calls, message passing, request-response flows.

**Layout pattern**: Vertical lifelines with horizontal message arrows.

```
Structure:
┌─ Outer Frame ─────────────────────────────────┐
│  ┌─Actor─┐  ┌─Actor─┐  ┌─Actor─┐             │
│  └───┬───┘  └───┬───┘  └───┬───┘             │
│      │          │          │                   │
│      │──── msg ▶│          │   (solid = sync)  │
│      │          │── msg ──▶│   (dashed = async)│
│      │          │◀── resp ─│                   │
│      │◀── resp ─│          │                   │
│      │          │          │                   │
│  ┌───┴──────────┴──────────┴───┐               │
│  │        Activation box       │               │
│  └─────────────────────────────┘               │
└────────────────────────────────────────────────┘
```

**Key elements**:
- Actor boxes at top (styled as cards with colored header)
- Vertical dashed lifelines extending down
- Horizontal arrows: solid for synchronous, dashed for asynchronous
- Return arrows: dashed with open arrowhead
- Activation boxes: narrow rectangles on lifelines
- Numbered steps as labels on arrows

## Entity-Relationship Diagram

**Use for**: Database schemas, data models, object relationships.

**Layout pattern**: Entity cards with relationship lines.

```
Structure:
┌─ Entity ──────┐      ┌─ Entity ──────┐
│ PK id         │──1:N─│ FK entity_id  │
│    name       │      │    field      │
│    created_at │      │    value      │
└───────────────┘      └───────────────┘
```

**Key elements**:
- Entity cards: header with table name, body with columns
- Column annotations: PK (bold), FK (italic), type (muted right-aligned)
- Relationship lines: labeled with cardinality (1:1, 1:N, N:M)
- Use diamond shapes for junction tables in M:N
- Group related entities visually

## Network / Topology Diagram

**Use for**: Infrastructure, Kubernetes clusters, network architecture.

**Layout pattern**: Nested zones with device/service nodes.

```
Structure:
┌─ VPC / Cluster ───────────────────────────────┐
│  ┌─ Subnet/Namespace ──────┐  ┌─ Subnet ────┐ │
│  │  (node) (node) (node)   │  │  (node)     │ │
│  │     │       │           │  │     │       │ │
│  └─────┼───────┼───────────┘  └─────┼───────┘ │
│        └───────┼─────────────────────┘         │
│  ┌─ Load Balancer ─────────────────────────┐   │
│  └─────────────────────────────────────────┘   │
│                    │                            │
│              ┌─ Internet ─┐                     │
└──────────────┴────────────┴─────────────────────┘
```

**Key elements**:
- Nested containers for zones/namespaces (lighter fill, dashed border)
- Device/service nodes as small cards or circles with icons
- Connection lines between nodes
- Firewall/LB as horizontal bars
- External endpoints at edges

## Comparison / Matrix Diagram

**Use for**: Feature comparison, decision matrix, side-by-side evaluation.

**Layout pattern**: Grid of cards with shared rows.

```
Structure:
┌─ Outer Frame ─────────────────────────────────┐
│           ┌─ Option A ─┐  ┌─ Option B ─┐      │
│  Feature  │  value     │  │  value     │      │
│  Feature  │  value     │  │  value     │      │
│  Feature  │  value     │  │  value     │      │
│           └────────────┘  └────────────┘      │
└────────────────────────────────────────────────┘
```

## Hierarchy / Org Chart

**Use for**: Organization charts, decision trees, inheritance.

**Layout pattern**: Top-down tree with parent-child connections.

```
Structure:
           ┌─ Root ─┐
           └───┬────┘
        ┌──────┼──────┐
   ┌─ Child ┐ ┌─ Child ┐ ┌─ Child ┐
   └───┬────┘ └────────┘ └───┬────┘
   ┌─ Leaf ┐              ┌─ Leaf ┐
   └───────┘              └───────┘
```

**Key elements**:
- Root node larger/more prominent
- Orthogonal connectors (right-angle lines)
- Consistent level spacing (80-100px between levels)
- Same-level nodes aligned horizontally
