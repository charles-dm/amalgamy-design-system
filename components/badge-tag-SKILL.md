# Component: Badge + Tag

## When to load this file
When displaying status indicators, categorical labels, count chips,
or dismissable filter chips on any screen.

## Figma source
Component sets: **Badge** (16 variants) · **Tag** (12 variants) · **Filter Chip** (6 variants)
File: R5IJ7h4lSqJq6smVlswkLQ · Page: 🧩 Components

---

## Badge

Passive, non-interactive label. Shows status or category. Never dismissable.

### Variants (Variant × Size)
| Variant | Color | When to use |
|---|---|---|
| `success` | green | Running, healthy, completed, online |
| `warning` | amber | Pending, degraded, approaching limit |
| `danger` | red | Failed, error, critical, offline |
| `info` | blue | Informational, queued (informational) |
| `queued` | slate-blue | Job in Slurm queue — position > 0 |
| `neutral` | gray | Unknown, inactive, archived |
| `teal` | teal | Amalgamy-specific state (late binding active, policy applied) |
| `aqua` | aqua | Secondary accent — use sparingly |

| Size | Height | Font | Use |
|---|---|---|---|
| `md` | 26px | 11px | Default |
| `sm` | 24px | 10px | Inside dense table cells |

### HTML anatomy

```html
<!-- Default -->
<span class="badge badge--success">Running</span>
<span class="badge badge--warning">Pending</span>
<span class="badge badge--danger">Failed</span>
<span class="badge badge--info">Queued</span>
<span class="badge badge--queued">Position 4</span>
<span class="badge badge--neutral">Inactive</span>
<span class="badge badge--teal">Late binding</span>

<!-- Small -->
<span class="badge badge--success badge--sm">Running</span>

<!-- With status dot (always pair color with text) -->
<span class="badge badge--success">
  <span class="status-dot status-dot--success" aria-hidden="true"></span>
  Running
</span>
```

### CSS

```css
.badge {
  display:        inline-flex;
  align-items:    center;
  gap:            var(--space-1);
  padding:        0 var(--space-2);
  height:         26px;
  font-family:    var(--font-mono);
  font-size:      var(--text-11);
  font-weight:    var(--weight-medium);
  letter-spacing: var(--tracking-caps-sm);
  text-transform: uppercase;
  border-radius:  var(--radius-sm);
  white-space:    nowrap;
  border:         1px solid transparent;
}

.badge--sm { height: 24px; font-size: var(--text-10); }

/* Variants */
.badge--success  { background: var(--color-success-bg);  color: var(--color-success-text);  border-color: var(--color-success-subtle); }
.badge--warning  { background: var(--color-warning-bg);  color: var(--color-warning-text);  border-color: var(--color-warning-subtle); }
.badge--danger   { background: var(--color-danger-bg);   color: var(--color-danger-text);   border-color: var(--color-danger-subtle); }
.badge--info     { background: var(--color-info-bg);     color: var(--color-info-text);     border-color: var(--color-info-subtle); }
.badge--queued   { background: var(--color-queued-bg);   color: var(--color-queued);        border-color: var(--color-queued); }
.badge--neutral  { background: var(--color-surface-2);   color: var(--color-text-tertiary); border-color: var(--color-border-subtle); }
.badge--teal     { background: var(--color-accent-muted-bg); color: var(--color-accent);    border-color: var(--color-accent-subtle); }
.badge--aqua     { background: var(--color-surface-2);   color: var(--color-aqua);          border-color: var(--color-aqua); }
```

---

## Tag

Interactive label. Used for filters, selected values, user-applied labels.
Can be dismissable (shows × button). Has hover state.

### Variants (Size × State × Dismissable)
| Property | Options |
|---|---|
| Size | `md` (29px), `sm` — match surrounding density |
| State | `default`, `hover`, `disabled` |
| Dismissable | `yes` (shows × button), `no` |

### HTML anatomy

```html
<!-- Non-dismissable -->
<span class="tag">GPU · A100</span>

<!-- Dismissable -->
<span class="tag tag--dismissable">
  <span class="tag__label">research-lab-a</span>
  <button class="tag__dismiss" type="button" aria-label="Remove research-lab-a">
    <svg class="tag__dismiss-icon" aria-hidden="true"><!-- x icon --></svg>
  </button>
</span>

<!-- Small -->
<span class="tag tag--sm tag--dismissable">
  <span class="tag__label">priority:high</span>
  <button class="tag__dismiss" type="button" aria-label="Remove priority:high">×</button>
</span>

<!-- Disabled -->
<span class="tag tag--disabled" aria-disabled="true">read-only</span>
```

### CSS

```css
.tag {
  display:        inline-flex;
  align-items:    center;
  gap:            var(--space-1);
  padding:        0 var(--space-2);
  height:         29px;
  font-family:    var(--font-sans);
  font-size:      var(--text-12);
  font-weight:    var(--weight-medium);
  color:          var(--color-text-primary);
  background:     var(--color-surface-2);
  border:         1px solid var(--color-border-default);
  border-radius:  var(--radius-sm);
  white-space:    nowrap;
  transition:     background var(--duration-base) var(--ease-out);
}
.tag:hover,
.tag--hover { background: var(--color-surface-3); }

.tag--sm { height: 24px; font-size: var(--text-11); }

.tag--disabled { opacity: 0.4; cursor: not-allowed; }

.tag__dismiss {
  display:     flex;
  align-items: center;
  padding:     0;
  color:       var(--color-text-tertiary);
  border:      none;
  background:  none;
  cursor:      pointer;
  transition:  color var(--duration-fast) var(--ease-out);
}
.tag__dismiss:hover { color: var(--color-text-primary); }
.tag__dismiss-icon  { width: 12px; height: 12px; }
```

---

## Filter Chip

For toggling active filters. Visually distinct from Tag — has a border and
active fill state. Used in filter bars above tables.

### HTML anatomy

```html
<!-- Filter bar wrapper -->
<div class="filter-bar" role="group" aria-label="Active filters">
  <button class="filter-chip" type="button" aria-pressed="false">
    All Nodes
  </button>
  <button class="filter-chip filter-chip--active" type="button" aria-pressed="true">
    Running
    <span class="filter-chip__count" aria-label="47 results">47</span>
  </button>
  <button class="filter-chip" type="button" aria-pressed="false">
    Failed
    <span class="filter-chip__count" aria-label="3 results">3</span>
  </button>
</div>
```

### CSS

```css
.filter-bar {
  display: flex;
  gap:     var(--space-2);
  flex-wrap: wrap;
}

.filter-chip {
  display:        inline-flex;
  align-items:    center;
  gap:            var(--space-2);
  padding:        0 var(--space-3);
  height:         30px;
  font-family:    var(--font-sans);
  font-size:      var(--text-12);
  font-weight:    var(--weight-medium);
  color:          var(--color-text-secondary);
  background:     transparent;
  border:         1px solid var(--color-border-default);
  border-radius:  var(--radius-md);
  cursor:         pointer;
  white-space:    nowrap;
  transition:     all var(--duration-base) var(--ease-out);
}
.filter-chip:hover {
  background:  var(--color-surface-2);
  color:       var(--color-text-primary);
}
.filter-chip--active,
.filter-chip[aria-pressed="true"] {
  background:   var(--color-accent-muted-bg);
  border-color: var(--color-accent);
  color:        var(--color-accent);
}
.filter-chip__count {
  font-family: var(--font-mono);
  font-size:   var(--text-11);
  color:       inherit;
  opacity:     0.8;
}
```

---

## Rules

- **Badge is passive. Tag is interactive.** If it does something when clicked, it's a Tag or Filter Chip, not a Badge.
- **Never use color alone to convey status.** Always pair a colored badge with a text label.
- **Queued is not warning.** `badge--queued` (slate-blue) is a distinct state for Slurm queue position. Use `badge--warning` for degraded or near-limit states.
- **Dismissable tags need `aria-label` on the dismiss button** naming what gets removed.
- **Filter chips use `aria-pressed`.** Screen readers announce toggle state.
