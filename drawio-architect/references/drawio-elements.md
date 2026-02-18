# Draw.io XML Element Reference

## Table of Contents
- [File Structure](#file-structure)
- [Common Style Properties](#common-style-properties)
- [Element Templates](#element-templates)
- [Arrow/Edge Templates](#arrowedge-templates)
- [Text & Label Formatting](#text--label-formatting)
- [Layout Coordinates](#layout-coordinates)
- [Unicode Icons](#unicode-icons)

---

## File Structure

Every `.drawio` file follows this structure:

```xml
<mxfile host="app.diagrams.net" modified="YYYY-MM-DDTHH:MM:SS.000Z" agent="Claude" version="24.0.0">
  <diagram name="Diagram Name" id="unique-id">
    <mxGraphModel dx="1422" dy="900" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="WIDTH" pageHeight="HEIGHT" math="0" shadow="1">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- All elements go here with parent="1" -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

Page sizes: Small=`850x600`, Medium=`1100x760`, Large=`1200x900`, XL=`1400x1000`.

## Common Style Properties

| Property | Values | Notes |
|----------|--------|-------|
| `rounded` | `0`/`1` | Round corners |
| `arcSize` | `1`-`50` | Corner radius (lower = subtle) |
| `fillColor` | `#hex` | Background color |
| `strokeColor` | `#hex` / `none` | Border color |
| `strokeWidth` | number | Border thickness |
| `fontColor` | `#hex` | Text color |
| `fontSize` | number | Text size in px |
| `fontStyle` | `0`=normal, `1`=bold, `2`=italic, `3`=bold+italic | — |
| `fontFamily` | `Helvetica`, `Courier New` | — |
| `align` | `left`/`center`/`right` | Horizontal text alignment |
| `verticalAlign` | `top`/`middle`/`bottom` | Vertical text alignment |
| `whiteSpace` | `wrap` | Enable text wrapping |
| `html` | `1` | Enable HTML in labels |
| `shadow` | `0`/`1` | Drop shadow |
| `dashed` | `0`/`1` | Dashed border |
| `dashPattern` | `N N` | e.g., `4 4` or `6 3` |
| `opacity` | `0`-`100` | Transparency |
| `spacingTop` | number | Top padding |
| `spacingLeft` | number | Left padding |
| `letterSpacing` | number | Character spacing |

## Element Templates

### Outer Frame (container)
```xml
<mxCell id="frame" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#F7F9FC;strokeColor=#B0BEC5;strokeWidth=2;arcSize=3;shadow=1;" vertex="1" parent="1">
  <mxGeometry x="40" y="30" width="1020" height="700" as="geometry" />
</mxCell>
```

### Title Bar
```xml
<mxCell id="title-bar" value="" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#37474F;strokeColor=none;" vertex="1" parent="1">
  <mxGeometry x="40" y="30" width="1020" height="50" as="geometry" />
</mxCell>
<mxCell id="title-text" value="&#x2638; Title" style="text;html=1;align=left;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=none;fillColor=none;fontColor=#FFFFFF;fontSize=18;fontStyle=1;fontFamily=Helvetica;" vertex="1" parent="1">
  <mxGeometry x="60" y="35" width="200" height="40" as="geometry" />
</mxCell>
```

### Section Label + Divider
```xml
<mxCell id="section-label" value="SECTION NAME" style="text;html=1;align=left;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=none;fillColor=none;fontColor=#78909C;fontSize=11;fontStyle=1;fontFamily=Helvetica;letterSpacing=2;" vertex="1" parent="1">
  <mxGeometry x="70" y="95" width="140" height="25" as="geometry" />
</mxCell>
<mxCell id="section-divider" value="" style="line;strokeWidth=1;strokeColor=#CFD8DC;fillColor=none;align=left;verticalAlign=middle;spacingTop=-1;rotatable=0;labelPosition=left;points=[];portConstraint=eastwest;" vertex="1" parent="1">
  <mxGeometry x="210" y="105" width="820" height="10" as="geometry" />
</mxCell>
```

### Card with Colored Header
```xml
<!-- Card body -->
<mxCell id="card" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#E3F2FD;strokeColor=#1565C0;strokeWidth=2;arcSize=5;shadow=1;" vertex="1" parent="1">
  <mxGeometry x="70" y="140" width="320" height="300" as="geometry" />
</mxCell>
<!-- Card header bar -->
<mxCell id="card-header" value="" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#1565C0;strokeColor=none;" vertex="1" parent="1">
  <mxGeometry x="70" y="140" width="320" height="50" as="geometry" />
</mxCell>
<!-- Icon + title inside header -->
<mxCell id="card-icon" value="&#x1F680;" style="text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=none;fillColor=none;fontSize=22;" vertex="1" parent="1">
  <mxGeometry x="78" y="145" width="40" height="40" as="geometry" />
</mxCell>
<mxCell id="card-title" value="Card Title" style="text;html=1;align=left;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=none;fillColor=none;fontColor=#FFFFFF;fontSize=16;fontStyle=1;" vertex="1" parent="1">
  <mxGeometry x="115" y="145" width="100" height="30" as="geometry" />
</mxCell>
```

### Simple Card (no header bar)
```xml
<mxCell id="simple-card" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#FFF8E1;strokeColor=#FFB300;strokeWidth=1.5;arcSize=8;shadow=1;" vertex="1" parent="1">
  <mxGeometry x="90" y="130" width="260" height="100" as="geometry" />
</mxCell>
```

### Inner Row / Field Box
```xml
<mxCell id="field-box" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#FFFFFF;strokeColor=#90CAF9;strokeWidth=1;arcSize=8;" vertex="1" parent="1">
  <mxGeometry x="90" y="220" width="280" height="40" as="geometry" />
</mxCell>
```

### Pill / Tag Badge
```xml
<mxCell id="pill" value="&#x2601; label" style="label;whiteSpace=wrap;html=1;rounded=1;arcSize=50;align=center;verticalAlign=middle;fillColor=#E1F5FE;strokeColor=#4FC3F7;strokeWidth=1;fontColor=#0277BD;fontSize=11;fontStyle=0;spacingLeft=6;spacingRight=6;" vertex="1" parent="1">
  <mxGeometry x="90" y="290" width="110" height="30" as="geometry" />
</mxCell>
```

### Port / Status Badge
```xml
<mxCell id="badge" value="Port 8080" style="label;whiteSpace=wrap;html=1;rounded=1;arcSize=50;align=center;verticalAlign=middle;fillColor=#2E7D32;strokeColor=none;fontColor=#FFFFFF;fontSize=11;fontStyle=1;spacingLeft=6;spacingRight=6;" vertex="1" parent="1">
  <mxGeometry x="135" y="350" width="120" height="28" as="geometry" />
</mxCell>
```

### Status Dot
```xml
<mxCell id="status-dot" value="" style="ellipse;whiteSpace=wrap;html=1;fillColor=#4CAF50;strokeColor=#2E7D32;strokeWidth=1;shadow=0;" vertex="1" parent="1">
  <mxGeometry x="350" y="428" width="14" height="14" as="geometry" />
</mxCell>
```

### Legend Bar
```xml
<mxCell id="legend" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ECEFF1;strokeColor=#B0BEC5;strokeWidth=1;arcSize=8;shadow=0;" vertex="1" parent="1">
  <mxGeometry x="70" y="640" width="960" height="55" as="geometry" />
</mxCell>
<!-- Color swatch in legend -->
<mxCell id="swatch" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#E3F2FD;strokeColor=#1565C0;strokeWidth=1;" vertex="1" parent="1">
  <mxGeometry x="155" y="655" width="16" height="16" as="geometry" />
</mxCell>
```

## Arrow/Edge Templates

### Solid Arrow (sequence)
```xml
<mxCell id="arrow" value="" style="endArrow=blockThin;endFill=1;html=1;strokeColor=#90A4AE;strokeWidth=2;" edge="1" parent="1" source="sourceId" target="targetId">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### Dashed Arrow (communication/dependency)
```xml
<mxCell id="arrow-dashed" value="" style="endArrow=blockThin;endFill=1;html=1;strokeColor=#90A4AE;strokeWidth=1.5;dashed=1;dashPattern=4 4;" edge="1" parent="1" source="sourceId" target="targetId">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### Colored Arrow (binding/routing)
```xml
<mxCell id="arrow-colored" value="" style="endArrow=blockThin;endFill=1;html=1;strokeColor=#1565C0;strokeWidth=2.5;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;" edge="1" parent="1" source="sourceId" target="targetId">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### Arrow with Waypoints
```xml
<mxCell id="arrow-waypoint" value="" style="endArrow=none;html=1;strokeColor=#1565C0;strokeWidth=1.5;dashed=1;dashPattern=3 3;" edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="540" y="545" as="sourcePoint" />
    <mxPoint x="230" y="480" as="targetPoint" />
    <Array as="points">
      <mxPoint x="540" y="510" />
      <mxPoint x="230" y="510" />
    </Array>
  </mxGeometry>
</mxCell>
```

### Arrow Label
```xml
<mxCell id="arrow-label" value="label text" style="text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=none;fillColor=#F7F9FC;fontColor=#78909C;fontSize=9;fontStyle=2;" vertex="1" parent="1">
  <mxGeometry x="660" y="323" width="40" height="20" as="geometry" />
</mxCell>
```

## Text & Label Formatting

### Standalone Text
```xml
<mxCell id="text" value="Text content" style="text;html=1;align=left;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=none;fillColor=none;fontColor=#37474F;fontSize=12;fontStyle=0;" vertex="1" parent="1">
  <mxGeometry x="100" y="200" width="100" height="25" as="geometry" />
</mxCell>
```

### HTML in Labels
Use `html=1` and HTML entities in `value`:
- Bold: `&lt;b&gt;text&lt;/b&gt;`
- Line break: `&lt;br&gt;`
- Colored span: `&lt;font style=&quot;color:#666&quot;&gt;text&lt;/font&gt;`
- Monospace: `&lt;font style=&quot;font-family: monospace;&quot;&gt;text&lt;/font&gt;`

## Layout Coordinates

- All positions are absolute (x, y from top-left of canvas)
- Use `mxGeometry` for positioning: `x`, `y`, `width`, `height`
- Grid snap: align to multiples of 10 for clean layouts
- Typical padding: 20-30px from container edge to content
- Typical gap between peer elements: 20-30px horizontal, 15-20px vertical

## Unicode Icons

Common icons for technical diagrams:

| Icon | Code | Use for |
|------|------|---------|
| ☸ | `&#x2638;` | Kubernetes |
| 🚀 | `&#x1F680;` | Deploy, launch, main app |
| 🤖 | `&#x1F916;` | Bot, agent, AI |
| 🔧 | `&#x1F527;` | Tools, settings |
| 📂 | `&#x1F4C2;` | Directory, workspace |
| 📄 | `&#x1F4C4;` | File, document |
| 🔑 | `&#x1F511;` | Auth, credentials, key |
| 💬 | `&#x1F4AC;` | Chat, messaging, Slack |
| 📦 | `&#x1F4E6;` | Package, module |
| 💾 | `&#x1F4BE;` | Storage, persistence |
| 🌐 | `&#x1F310;` | Web, browser, network |
| 🔍 | `&#x1F50D;` | Search, debug |
| ⚙ | `&#x2699;` | Config, settings |
| ☁ | `&#x2601;` | Cloud |
| ✈ | `&#x2708;` | Telegram |
| 🧩 | `&#x1F9E9;` | Plugin, skill, extension |
| 📍 | `&#x1F4CD;` | Location, mount point |
| ✅ | `&#x2705;` | Success, enabled |
| 🔒 | `&#x1F512;` | Locked, read-only |
| ⚠️ | `&#x26A0;&#xFE0F;` | Warning, restricted |
| 🗃 | `&#x1F5C3;` | Database |
