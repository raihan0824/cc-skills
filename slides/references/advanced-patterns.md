# Advanced Slide Patterns

## Diagram Patterns

### Flow Diagram (CSS-only)
```html
<div class="flow-diagram">
    <div class="flow-step">
        <div class="flow-icon">1</div>
        <div class="flow-label">Input</div>
    </div>
    <div class="flow-arrow">→</div>
    <div class="flow-step">
        <div class="flow-icon">2</div>
        <div class="flow-label">Process</div>
    </div>
    <div class="flow-arrow">→</div>
    <div class="flow-step">
        <div class="flow-icon">3</div>
        <div class="flow-label">Output</div>
    </div>
</div>

<style>
.flow-diagram {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
}
.flow-step {
    text-align: center;
}
.flow-icon {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background: var(--primary-color);
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.5rem;
    font-weight: bold;
    margin-bottom: 0.5rem;
}
.flow-label {
    font-size: 0.9rem;
    color: var(--text-muted);
}
.flow-arrow {
    font-size: 2rem;
    color: var(--text-muted);
}
</style>
```

### Architecture Diagram
```html
<div class="architecture">
    <div class="arch-layer">
        <span class="arch-label">Client</span>
        <div class="arch-boxes">
            <div class="arch-box">Web App</div>
            <div class="arch-box">Mobile</div>
        </div>
    </div>
    <div class="arch-connector">↓</div>
    <div class="arch-layer">
        <span class="arch-label">API</span>
        <div class="arch-boxes">
            <div class="arch-box highlight">Gateway</div>
        </div>
    </div>
    <div class="arch-connector">↓</div>
    <div class="arch-layer">
        <span class="arch-label">Services</span>
        <div class="arch-boxes">
            <div class="arch-box">Auth</div>
            <div class="arch-box">Data</div>
            <div class="arch-box">Cache</div>
        </div>
    </div>
</div>

<style>
.architecture {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
}
.arch-layer {
    display: flex;
    align-items: center;
    gap: 1rem;
    width: 100%;
}
.arch-label {
    width: 80px;
    font-weight: bold;
    color: var(--text-muted);
}
.arch-boxes {
    display: flex;
    gap: 0.5rem;
    flex: 1;
}
.arch-box {
    background: var(--bg-secondary);
    padding: 0.75rem 1.5rem;
    border-radius: 4px;
    border: 1px solid var(--primary-color);
}
.arch-box.highlight {
    background: var(--primary-color);
    color: white;
}
.arch-connector {
    font-size: 1.5rem;
    color: var(--text-muted);
}
</style>
```

### Comparison Table
```html
<div class="comparison-table">
    <div class="compare-row header">
        <div class="compare-cell"></div>
        <div class="compare-cell">Before</div>
        <div class="compare-cell highlight">After</div>
    </div>
    <div class="compare-row">
        <div class="compare-cell label">Latency</div>
        <div class="compare-cell bad">500ms</div>
        <div class="compare-cell good">50ms</div>
    </div>
    <div class="compare-row">
        <div class="compare-cell label">Throughput</div>
        <div class="compare-cell bad">100 req/s</div>
        <div class="compare-cell good">10K req/s</div>
    </div>
</div>

<style>
.comparison-table {
    width: 100%;
    max-width: 600px;
    margin: 0 auto;
}
.compare-row {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 1px;
    background: var(--bg-secondary);
}
.compare-row.header {
    font-weight: bold;
}
.compare-cell {
    padding: 1rem;
    background: var(--bg-color);
    text-align: center;
}
.compare-cell.label {
    text-align: left;
    font-weight: 500;
}
.compare-cell.highlight {
    background: var(--primary-color);
    color: white;
}
.compare-cell.good {
    color: #22c55e;
}
.compare-cell.bad {
    color: #ef4444;
}
</style>
```

## Animation Patterns

### Token Streaming Effect
```html
<div class="streaming-text">
    <span class="token">Hello</span>
    <span class="token">world,</span>
    <span class="token">this</span>
    <span class="token">is</span>
    <span class="token">streaming</span>
    <span class="token">text.</span>
</div>

<style>
.streaming-text {
    font-size: 1.5rem;
    line-height: 2;
}
.token {
    opacity: 0;
    animation: tokenAppear 0.15s ease-out forwards;
}
.token:nth-child(1) { animation-delay: 0.1s; }
.token:nth-child(2) { animation-delay: 0.25s; }
.token:nth-child(3) { animation-delay: 0.4s; }
.token:nth-child(4) { animation-delay: 0.55s; }
.token:nth-child(5) { animation-delay: 0.7s; }
.token:nth-child(6) { animation-delay: 0.85s; }

@keyframes tokenAppear {
    from { opacity: 0; transform: translateY(5px); }
    to { opacity: 1; transform: translateY(0); }
}
</style>
```

### Progress/Loading Animation
```html
<div class="progress-bar">
    <div class="progress-fill"></div>
</div>

<style>
.progress-bar {
    width: 100%;
    height: 8px;
    background: var(--bg-secondary);
    border-radius: 4px;
    overflow: hidden;
}
.progress-fill {
    height: 100%;
    width: 0;
    background: linear-gradient(90deg, var(--primary-color), var(--accent-color));
    animation: fillProgress 2s ease-out forwards;
}
@keyframes fillProgress {
    to { width: 75%; }
}
</style>
```

### Failover Animation
```html
<div class="failover-demo">
    <div class="server primary">
        <div class="server-status"></div>
        <span>Primary</span>
    </div>
    <div class="traffic-line"></div>
    <div class="server backup">
        <div class="server-status"></div>
        <span>Backup</span>
    </div>
</div>

<style>
.failover-demo {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 2rem;
}
.server {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
}
.server-status {
    width: 60px;
    height: 60px;
    border-radius: 8px;
    background: #22c55e;
}
.server.primary .server-status {
    animation: serverFail 3s ease-in-out infinite;
}
.server.backup .server-status {
    animation: serverActivate 3s ease-in-out infinite;
}
.traffic-line {
    width: 100px;
    height: 4px;
    background: var(--primary-color);
    animation: trafficShift 3s ease-in-out infinite;
}

@keyframes serverFail {
    0%, 40% { background: #22c55e; }
    50%, 100% { background: #ef4444; }
}
@keyframes serverActivate {
    0%, 40% { background: #6b7280; }
    50%, 100% { background: #22c55e; }
}
@keyframes trafficShift {
    0%, 40% { transform: translateY(0); }
    50%, 100% { transform: translateY(40px); }
}
</style>
```

## Layout Patterns

### Split Screen with Gradient
```html
<div class="slide split-gradient">
    <div class="split-left">
        <h2>Left Content</h2>
        <p>Description text here</p>
    </div>
    <div class="split-right">
        <img src="image.png" class="slide-image">
    </div>
</div>

<style>
.split-gradient {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0;
}
.split-left {
    background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
    color: white;
    padding: 3rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
}
.split-right {
    padding: 2rem;
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>
```

### Card Grid
```html
<div class="card-grid">
    <div class="card">
        <div class="card-icon">🚀</div>
        <h3>Feature 1</h3>
        <p>Description</p>
    </div>
    <div class="card">
        <div class="card-icon">⚡</div>
        <h3>Feature 2</h3>
        <p>Description</p>
    </div>
    <div class="card">
        <div class="card-icon">🔒</div>
        <h3>Feature 3</h3>
        <p>Description</p>
    </div>
</div>

<style>
.card-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1.5rem;
}
.card {
    background: var(--bg-secondary);
    padding: 1.5rem;
    border-radius: 8px;
    text-align: center;
}
.card-icon {
    font-size: 2.5rem;
    margin-bottom: 1rem;
}
.card h3 {
    margin-bottom: 0.5rem;
}
.card p {
    color: var(--text-muted);
    font-size: 0.9rem;
}
</style>
```

### Timeline
```html
<div class="timeline">
    <div class="timeline-item">
        <div class="timeline-dot"></div>
        <div class="timeline-content">
            <span class="timeline-date">Q1 2024</span>
            <h4>Phase 1</h4>
            <p>Initial development</p>
        </div>
    </div>
    <div class="timeline-item">
        <div class="timeline-dot"></div>
        <div class="timeline-content">
            <span class="timeline-date">Q2 2024</span>
            <h4>Phase 2</h4>
            <p>Beta release</p>
        </div>
    </div>
</div>

<style>
.timeline {
    position: relative;
    padding-left: 2rem;
}
.timeline::before {
    content: '';
    position: absolute;
    left: 7px;
    top: 0;
    bottom: 0;
    width: 2px;
    background: var(--primary-color);
}
.timeline-item {
    position: relative;
    margin-bottom: 2rem;
}
.timeline-dot {
    position: absolute;
    left: -2rem;
    top: 0;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: var(--primary-color);
}
.timeline-date {
    font-size: 0.8rem;
    color: var(--primary-color);
    font-weight: bold;
}
.timeline-content h4 {
    margin: 0.25rem 0;
}
.timeline-content p {
    color: var(--text-muted);
    margin: 0;
}
</style>
```

## Chart Patterns

Charts animate when the slide becomes active. Use `data-target` for animated counters.

### Bar Chart (Animated)
```html
<div class="bar-chart">
    <div class="bar-item">
        <div class="bar-value">85%</div>
        <div class="bar" style="height: 85%;"></div>
        <div class="bar-label">Q1</div>
    </div>
    <div class="bar-item">
        <div class="bar-value">92%</div>
        <div class="bar" style="height: 92%;"></div>
        <div class="bar-label">Q2</div>
    </div>
    <div class="bar-item">
        <div class="bar-value">78%</div>
        <div class="bar" style="height: 78%;"></div>
        <div class="bar-label">Q3</div>
    </div>
</div>
```
Bars grow from bottom when slide activates. Set height as percentage of max value.

### Animated Counter
```html
<div class="metric">
    <div class="metric-value" data-target="99.9">0</div>
    <div class="metric-label">% Uptime</div>
</div>
```
Counter animates from 0 to target value with easing. Supports decimals.

### Donut Chart
```html
<div class="donut-chart" style="background: conic-gradient(
    var(--primary-color) 0deg 216deg,
    var(--accent-color) 216deg 288deg,
    var(--bg-secondary) 288deg 360deg
);">
    <div class="donut-center">
        <div class="donut-value">60%</div>
        <div class="donut-label">Complete</div>
    </div>
</div>
```
Use conic-gradient for segments. 360deg = 100%, so 60% = 216deg.

### Line Chart (SVG with draw animation)
```html
<div class="line-chart">
    <svg viewBox="0 0 400 200">
        <path class="line" d="M 0 150 L 100 120 L 200 80 L 300 100 L 400 40" />
    </svg>
</div>
```
Line draws itself when slide activates. Adjust path points for your data.

### Horizontal Bar Chart
```html
<div class="h-bar-chart">
    <div class="h-bar-item">
        <div class="h-bar-label">Category A</div>
        <div class="h-bar-track">
            <div class="h-bar-fill" style="--target-width: 85%;"></div>
        </div>
        <div class="h-bar-value">85%</div>
    </div>
</div>

<style>
.h-bar-chart {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}
.h-bar-item {
    display: grid;
    grid-template-columns: 120px 1fr 60px;
    align-items: center;
    gap: 1rem;
}
.h-bar-track {
    height: 24px;
    background: var(--bg-secondary);
    border-radius: 4px;
    overflow: hidden;
}
.h-bar-fill {
    height: 100%;
    width: 0;
    background: linear-gradient(90deg, var(--primary-color), var(--accent-color));
    border-radius: 4px;
}
.slide.active .h-bar-fill {
    animation: growWidth 0.8s ease-out forwards;
}
@keyframes growWidth {
    to { width: var(--target-width); }
}
</style>
```

### Comparison Bars (Before/After)
```html
<div class="compare-bars">
    <div class="compare-item">
        <div class="compare-label">Latency</div>
        <div class="compare-values">
            <div class="before-bar">
                <span>500ms</span>
                <div class="bar-bg bad" style="width: 100%;"></div>
            </div>
            <div class="after-bar">
                <span>50ms</span>
                <div class="bar-bg good" style="width: 10%;"></div>
            </div>
        </div>
    </div>
</div>

<style>
.compare-bars { display: flex; flex-direction: column; gap: 2rem; }
.compare-item { display: flex; flex-direction: column; gap: 0.5rem; }
.compare-values { display: flex; flex-direction: column; gap: 0.25rem; }
.before-bar, .after-bar {
    display: flex; align-items: center; gap: 1rem;
}
.before-bar span { color: #ef4444; min-width: 80px; }
.after-bar span { color: #22c55e; min-width: 80px; }
.bar-bg { height: 20px; border-radius: 4px; }
.bar-bg.bad { background: #ef4444; }
.bar-bg.good { background: #22c55e; }
</style>
```
