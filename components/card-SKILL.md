# Component: Card

## When to load this file
When building any contained surface — metric cards, detail panels,
empty states, modals, or any box that groups related content.

## Figma source
Component set: **Card** (16 variants: Style × State)
File: R5IJ7h4lSqJq6smVlswkLQ · Page: 🧩 Components

---

## Variants

### Style
| Value | When |
|---|---|
| `default` | Most cards — surface/1 background, subtle border |
| `outlined` | When on a lighter surface (surface/2 context) — stronger border |
| `filled` | Accent-filled state — selected card, active metric highlight |
| `elevated` | Floating element — dropdown, popover, modal. Adds shadow-md. |

### State
| Value | When |
|---|---|
| `default` | Static display — metric card, info panel |
| `hover` | Interactive card — clickable row card, selectable item |
| `selected` | Active selection — current item in list+detail |
| `disabled` | Non-interactive, unavailable resource |

---

## HTML anatomy

### Metric card (operator dashboard)
```html
<article class="card" aria-label="GPU Utilization">
  <header class="card__header">
    <span class="type-heading-card-sm text-secondary">GPU Utilization</span>
    <span class="badge badge--success">Live</span>
  </header>
  <div class="card__body">
    <span class="data-value data-value--hero">94.2%</span>
  </div>
  <footer class="card__footer">
    <span class="type-body-xs text-tertiary">Last updated 4s ago</span>
  </footer>
</article>
```

### Detail panel card
```html
<section class="card card--outlined">
  <header class="card__header">
    <h2 class="type-heading-section">Node details</h2>
    <button class="btn btn--outlined btn--sm">Edit</button>
  </header>
  <div class="card__body card__body--pairs">
    <dl class="detail-pairs">
      <div class="detail-pair">
        <dt class="type-heading-xs text-secondary">Node ID</dt>
        <dd class="data-value">a100-node-07</dd>
      </div>
      <div class="detail-pair">
        <dt class="type-heading-xs text-secondary">Status</dt>
        <dd><span class="badge badge--success">Running</span></dd>
      </div>
      <div class="detail-pair">
        <dt class="type-heading-xs text-secondary">GPU util</dt>
        <dd class="data-value">94.2%</dd>
      </div>
      <div class="detail-pair">
        <dt class="type-heading-xs text-secondary">Active jobs</dt>
        <dd class="data-value">12</dd>
      </div>
    </dl>
  </div>
</section>
```

### Clickable / selectable card
```html
<button class="card card--interactive" type="button"
        aria-pressed="false">
  <header class="card__header">
    <span class="type-heading-card-sm">research-lab-a</span>
    <span class="badge badge--teal">Active</span>
  </header>
  <div class="card__body">
    <p class="type-body-sm text-secondary">
      48 active jobs · 2,841 GPU-hrs this month
    </p>
  </div>
</button>

<!-- Selected state -->
<button class="card card--interactive card--selected" type="button"
        aria-pressed="true">
  <!-- same content -->
</button>
```

### Alert / notice card
```html
<div class="card card--alert card--alert-warning" role="alert">
  <div class="card__alert-icon">
    <svg aria-hidden="true"><!-- warning icon --></svg>
  </div>
  <div class="card__alert-body">
    <p class="type-heading-group">GPU memory pressure detected</p>
    <p class="type-body-sm text-secondary">
      a100-node-07 is at 89% memory. Jobs may fail if usage exceeds 90%.
    </p>
  </div>
  <button class="btn btn--outlined btn--sm">View node</button>
</div>
```

### Modal card
```html
<div class="modal-overlay" role="dialog" aria-modal="true"
     aria-labelledby="modal-title">
  <div class="card card--elevated card--modal">
    <header class="card__header">
      <h2 class="type-heading-section" id="modal-title">Confirm deletion</h2>
      <button class="btn btn--outlined btn--sm btn--icon-only"
              aria-label="Close dialog">
        <svg class="btn__icon" aria-hidden="true"><!-- x --></svg>
      </button>
    </header>
    <div class="card__body">
      <p class="type-body-md text-secondary">
        This will permanently delete policy <strong>gpu-limit-v2</strong>.
        This action cannot be undone.
      </p>
    </div>
    <footer class="card__footer card__footer--actions">
      <button class="btn btn--secondary" type="button">Cancel</button>
      <button class="btn btn--destructive" type="button">Delete policy</button>
    </footer>
  </div>
</div>
```

---

## CSS

```css
/* Base card */
.card {
  background:    var(--color-surface-1);
  border:        1px solid var(--color-border-subtle);
  border-radius: var(--radius-lg);
  display:       flex;
  flex-direction: column;
}

/* Styles */
.card--outlined {
  border-color: var(--color-border-default);
}
.card--filled {
  background:  var(--color-accent-muted-bg);
  border-color: var(--color-accent-subtle);
}
.card--elevated {
  background:  var(--color-surface-1);
  border-color: var(--color-border-default);
  box-shadow:  var(--shadow-md);
}

/* States */
.card--interactive {
  cursor:     pointer;
  text-align: left;
  width:      100%;
  transition: background var(--duration-base) var(--ease-out),
              border-color var(--duration-base) var(--ease-out);
}
.card--interactive:hover {
  background:  var(--color-surface-2);
  border-color: var(--color-border-default);
}
.card--selected,
.card--interactive[aria-pressed="true"] {
  background:  var(--color-accent-muted-bg);
  border-color: var(--color-accent);
}
.card--disabled {
  opacity: 0.5;
  cursor:  not-allowed;
  pointer-events: none;
}

/* Card regions */
.card__header {
  display:     flex;
  align-items: center;
  justify-content: space-between;
  gap:         var(--space-3);
  padding:     var(--space-4) var(--space-5);
  border-bottom: 1px solid var(--color-border-subtle);
}
.card__body { padding: var(--space-5); flex: 1; }
.card__body--pairs { padding: var(--space-4) var(--space-5); }
.card__footer {
  padding: var(--space-3) var(--space-5);
  border-top: 1px solid var(--color-border-subtle);
}
.card__footer--actions {
  display: flex;
  justify-content: flex-end;
  gap: var(--space-3);
  padding: var(--space-4) var(--space-5);
}

/* Detail pairs */
.detail-pairs { display: grid; grid-template-columns: repeat(2, 1fr); gap: var(--space-4); }
.detail-pair  { display: flex; flex-direction: column; gap: 2px; }

/* Alert card */
.card--alert {
  flex-direction: row;
  align-items:    flex-start;
  gap:            var(--space-4);
  padding:        var(--space-4) var(--space-5);
  border-left-width: 3px;
}
.card--alert-warning { border-left-color: var(--color-warning); background: var(--color-warning-bg); }
.card--alert-danger  { border-left-color: var(--color-danger);  background: var(--color-danger-bg);  }
.card--alert-info    { border-left-color: var(--color-info);    background: var(--color-info-bg);    }
.card--alert-success { border-left-color: var(--color-success); background: var(--color-success-bg); }
.card__alert-icon    { flex-shrink: 0; width: 20px; height: 20px; }
.card__alert-body    { flex: 1; display: flex; flex-direction: column; gap: var(--space-1); }

/* Modal */
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex; align-items: center; justify-content: center;
  z-index: 60;
  padding: var(--space-5);
}
.card--modal {
  width: 100%;
  max-width: 480px;
  box-shadow: var(--shadow-lg);
}
```

---

## Dashboard grid

```html
<!-- 4-across metric cards -->
<div class="dashboard-grid">
  <article class="card"><!-- GPU util --></article>
  <article class="card"><!-- kW/token --></article>
  <article class="card"><!-- Active jobs --></article>
  <article class="card"><!-- Alerts --></article>
</div>
```

```css
.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: var(--space-4);
}
@media (max-width: 1024px) {
  .dashboard-grid { grid-template-columns: repeat(2, 1fr); }
}
```

---

## Rules

- **One `data-value--hero` per card.** The big number is the card's headline.
- **Card header is optional.** Don't add one just to have a header.
- **`role="alert"` on error/warning cards** that appear dynamically.
- **Modal cards need `role="dialog"` and `aria-modal="true"`** on the overlay.
- **Never nest a card inside a card** more than one level deep. Use `.detail-pairs` instead.
- **Interactive card uses `<button>` not `<div>`.** Keyboard and screen reader users need it.
