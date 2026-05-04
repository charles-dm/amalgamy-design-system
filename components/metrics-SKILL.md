---
name: amalgamy-metrics
description: Metric display components for Amalgamy LaunchHPC prototypes — KPI tiles, big-number readouts, sparkline-decorated cells, and the four-up metric grid. Use this skill whenever the user needs to display utilization, throughput, queue depth, SLA, capacity, or any other quantitative system metric. Also use for "headline number" displays (a single large value with context). Depends on the amalgamy-sovereign-terminal foundation skill — load that first.
---

# Metric Displays

The most-used component family in any operations or executive prototype for Amalgamy. Two scales (display and value), three layouts (four-up grid, single readout, status-row variant), and a small set of decorations (sparkline, trend delta, unit suffix).

## When to use what

| Pattern | When |
|---------|------|
| Four-up metric grid | Operations dashboards. Top-of-page KPI strip showing 4 related measures (e.g., GPU util · jobs · queue · SLA). The standard. |
| Two-up or three-up grid | Executive screens. Fewer, larger metrics per panel. |
| Single headline metric | Hero displays, narrative screens, drill-down detail pages. Use `role-metric-display` at full size. |
| Status row | When the metric *is* a state count (running/healthy/warn/crit/idle). Use the status-row variant — see foundation patterns doc. |

## The four-up metric grid (canonical)

This is the default. Treat it as the recipe; depart with reason.

### Structure

A 4-column grid wrapped in a single panel border, with internal cell dividers:

```html
<div class="metric-grid">
  <div class="metric-cell">
    <div class="lbl">GPU UTIL <span class="tag">cluster-a</span></div>
    <div class="val">97.4<span class="u">%</span></div>
    <div class="trend"><span class="delta up">+2.1pp</span><span>VS LAST WEEK</span></div>
    <svg class="spark" viewBox="0 0 60 18" preserveAspectRatio="none">
      <path d="M0 14 L8 12 L14 13 L20 9 L26 10 L32 6 L38 7 L44 4 L50 5 L60 2"/>
    </svg>
  </div>
  <!-- 3 more cells -->
</div>
```

### Token bindings

```css
.metric-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0;                                       /* cells share borders, no gap */
  border: 1px solid var(--color-border-strong);
}

.metric-cell {
  padding: 22px 24px 20px;
  background: var(--color-surface-1);
  border-right: 1px solid var(--color-border-strong);
  position: relative;                           /* for sparkline absolute pos */
}
.metric-cell:last-child { border-right: none; }

.metric-cell .lbl {
  /* role-label baseline + flex layout for optional tag */
  font-family: var(--role-label-family);
  font-size: var(--role-label-size);
  font-weight: var(--role-label-weight);
  letter-spacing: var(--letter-spacing-wide-2);
  text-transform: uppercase;
  color: var(--color-text-tertiary);
  margin-bottom: 18px;
  display: flex;
  justify-content: space-between;
}
.metric-cell .lbl .tag { color: var(--color-accent-subtle); }

.metric-cell .val {
  /* role-metric-display, but composed inline because of the inline unit */
  font-family: var(--role-metric-display-family);
  font-size: var(--role-metric-display-size);
  font-weight: var(--role-metric-display-weight);
  line-height: var(--role-metric-display-line-height);
  letter-spacing: var(--role-metric-display-letter-spacing);
  font-variant-numeric: tabular-nums;
  color: var(--color-text-primary);
}
.metric-cell .val .u {
  font-size: 18px;                              /* ~38% of metric-display size */
  color: var(--color-text-secondary);
  margin-left: 2px;
  font-weight: var(--font-weight-regular);
}

.metric-cell .trend {
  margin-top: 12px;
  font-family: var(--role-secondary-family);
  font-size: var(--font-size-10);
  letter-spacing: var(--letter-spacing-wide-1);
  color: var(--color-text-tertiary);
  display: flex;
  gap: 6px;
  align-items: center;
}
.metric-cell .delta {
  font-weight: var(--font-weight-semibold);
}
.metric-cell .delta.up   { color: var(--color-success-text); }
.metric-cell .delta.dn   { color: var(--color-danger-text); }
.metric-cell .delta.warn { color: var(--color-warning-text); }
.metric-cell .delta.flat { color: var(--color-text-tertiary); }

.metric-cell .spark {
  position: absolute;
  right: 14px;
  bottom: 14px;
  width: 60px;
  height: 18px;
}
.metric-cell .spark path {
  fill: none;
  stroke: var(--color-accent);
  stroke-width: 1.25;
  opacity: 0.7;
}
```

### Anatomy

A metric cell carries up to four pieces of information, in this exact order top-to-bottom:

1. **Label** (required) — uppercase, tertiary text, optional teal-deep tag on the right specifying the slice (cluster, time window, percentile)
2. **Value** (required) — large tabular-nums number, optional inline unit at ~38% size in secondary text color
3. **Trend** (optional) — small line: colored delta + uppercase comparison context. Skip if there's no meaningful comparison.
4. **Sparkline** (optional decoration) — absolute-positioned in the bottom-right, ~60×18px, single teal stroke at 70% opacity. Decorative; not the primary signal.

Cells should be visually consistent within a grid — if one has a sparkline, all should. If one shows a trend, all should.

## Headline metric (single readout)

For hero or detail screens where one number matters most.

```html
<div class="metric-headline">
  <div class="lbl">FLEET CAPACITY <span class="tag">30d trailing</span></div>
  <div class="val role-metric-display">847,290<span class="u">gpu-hr</span></div>
  <div class="trend"><span class="delta up">+18.2%</span><span>VS PREVIOUS 30D</span></div>
</div>
```

Same anatomy, no grid, no border. Sized larger via the `role-metric-display` class on the value. Place inside a panel with `--color-surface-1` background and 1px border to match other prototype elements.

## Conventions

### Numbers

- Always tabular-nums. The `role-metric-display` and `role-metric-value` utilities bake this in; if you compose a custom numeric style add it explicitly.
- Use locale formatting with thousands separators (`18,402` not `18402`).
- Decimal precision should match what the metric semantically supports — `97.4%` for utilization (1 decimal), `99.98%` for SLA (2 decimals because the 9s matter), integer counts unitless, durations in human format (`00:04:12`, `1.2s`, `14d`).
- Express small percentage deltas in **percentage points** (`+2.1pp`) not percent — the distinction matters for SLA-style metrics.

### Units

The unit-as-suffix pattern (`97.4%`, `512gpu`, `18ms`) reads cleanly because the unit is rendered visibly smaller and lighter than the value. Don't break this with a space — the unit hugs the number. The exception is multi-word units (`gpu-hr`, `req/s`) which can use a thin gap.

### Deltas

Color is the primary signal:
- **`up` (success-text)** for "good direction" — utilization rising, SLA holding, jobs completing
- **`dn` (danger-text)** for "bad direction" — failures up, SLA dropping, capacity falling
- **`warn` (warning-text)** for "watch this" — queue depth growing, latency rising, anything trending toward a threshold
- **`flat` (text-tertiary)** for stable / no-change

Note that "up" doesn't always mean increasing. SLA *dropping* by 0.01pp is a `dn` delta even though numerically it's `−0.01pp`. Color follows the **product interpretation**, not the math sign.

### Sparklines

- Always 60×18 viewBox, `preserveAspectRatio="none"` so the SVG fills its allocated box.
- 8–10 data points is the right density. Fewer reads as too coarse; more reads as noise.
- Single stroke, no fill, no axis, no labels. Decorative trend cue, not a real chart.
- Stroke color is `--color-accent` at 0.7 opacity. Never status-colored — sparklines convey shape, not state. State is in the delta text.

### Skipping decoration

Don't feel obligated to include trend or sparkline on every metric. A capacity figure on a detail page may not have a trend. A current-instant counter (queue depth right now) may not have a sparkline. Prefer to omit than to invent.

## Variants observed in the canonical style tile

The `examples/sovereign-terminal-style-tile.html` shows the four-up grid with all decorations active. Reference cells:

- `GPU Util / cluster-a` → `97.4%` → `+2.1pp VS LAST WEEK` → ascending sparkline
- `Jobs / 24h` → `18,402` → `+312 VS YESTERDAY` → ascending sparkline
- `Queue / p95` → `1,247` → `+184 VS LAST HOUR` (warn) → choppy sparkline
- `SLA / 30d` → `99.98%` → `−0.01pp VS 30D` (dn) → flat sparkline

The pattern of "metric · slice tag" in the label and "comparison value · comparison window" in the trend is repeated across all four. That parallelism is part of the language — don't randomize the structure across cells in the same grid.

## Common mistakes

- **Putting the unit inline at full size** (`97.4 %` with the % the same size as the number). The unit must be smaller and dimmer.
- **Using teal for delta colors.** Deltas use the status palette (success/danger/warning), not the brand palette.
- **Sparklines in status colors.** Sparklines are decorative trend cues, always teal.
- **Mixing densities of decoration in one grid.** All cells get sparklines or none do. All cells show trends or none do.
- **Box shadows on metric cells.** Never. The 1px outer border + cell dividers is the entire visual frame.
- **Rounded corners on the grid or cells.** Hard edges only.
- **Inter on the value.** Always JetBrains Mono via `role-metric-display`.

## Compact-density behavior

When `data-density="compact"` is active, the multiplier handles padding and overall rhythm automatically. The metric value itself does not shrink — at compact density the visual hierarchy *increases* (the metric becomes more prominent relative to surrounding chrome) which is the desired effect for power-user screens.

## Spacious-density behavior

At `data-density="spacious"` the cell padding grows substantially. Consider switching from a four-up to a two-up or three-up grid at this density — four cells with spacious padding can blow past the 1280px content width.
