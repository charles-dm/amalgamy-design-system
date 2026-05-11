# Component: Feedback — Alerts, Toasts, Tooltips, Empty States

## When to load this file
When building any screen that needs to communicate system status,
async action results, inline validation, helper text, or zero-item states.

## Figma source
Component sets: **Tooltip** (1 variant) · **List Item** (32 variants)
Alert and Toast are assembled from Card + tokens — no dedicated component set.
File: R5IJ7h4lSqJq6smVlswkLQ · Page: 🧩 Components

---

## Inline alert

Static, embedded in the page layout. Does not dismiss automatically.
Use for: form validation errors, warnings about irreversible actions,
system notices relevant to the current page.

```html
<!-- Warning -->
<div class="alert alert--warning" role="alert">
  <svg class="alert__icon" aria-hidden="true"><!-- warning triangle --></svg>
  <div class="alert__body">
    <p class="alert__title type-heading-group">Memory limit approaching</p>
    <p class="alert__desc type-body-sm text-secondary">
      Node a100-07 is at 89% memory. Jobs may fail above 90%.
    </p>
  </div>
  <a class="btn btn--outlined btn--sm" href="/nodes/a100-07">View node</a>
</div>

<!-- Danger -->
<div class="alert alert--danger" role="alert">
  <svg class="alert__icon" aria-hidden="true"><!-- x-circle --></svg>
  <div class="alert__body">
    <p class="alert__title type-heading-group">Job failed</p>
    <p class="alert__desc type-body-sm text-secondary">
      hpc-job-2891 exited with code 137 (OOM). Review the Decision Ledger for details.
    </p>
  </div>
  <a class="btn btn--outlined btn--sm" href="/ledger/2891">View ledger</a>
</div>

<!-- Info -->
<div class="alert alert--info" role="status">
  <svg class="alert__icon" aria-hidden="true"><!-- info --></svg>
  <div class="alert__body">
    <p class="alert__desc type-body-sm text-secondary">
      LaunchHPC rerouted this job to a100-07 (data already local). Saved ~14 min.
    </p>
  </div>
</div>

<!-- Success -->
<div class="alert alert--success" role="status">
  <svg class="alert__icon" aria-hidden="true"><!-- check-circle --></svg>
  <div class="alert__body">
    <p class="alert__desc type-body-sm">Job submitted successfully — position 4 in queue.</p>
  </div>
</div>
```

```css
.alert {
  display:    flex;
  align-items: flex-start;
  gap:        var(--space-3);
  padding:    var(--space-4);
  border:     1px solid;
  border-left-width: 3px;
  border-radius: var(--radius-md);
}
.alert__icon  { flex-shrink: 0; width: 18px; height: 18px; margin-top: 1px; }
.alert__body  { flex: 1; display: flex; flex-direction: column; gap: 2px; }
.alert__title { margin: 0; }
.alert__desc  { margin: 0; }

.alert--warning {
  background:       var(--color-warning-bg);
  border-color:     var(--color-warning-subtle);
  border-left-color: var(--color-warning);
  color:            var(--color-warning-text);
}
.alert--danger {
  background:       var(--color-danger-bg);
  border-color:     var(--color-danger-subtle);
  border-left-color: var(--color-danger);
  color:            var(--color-danger-text);
}
.alert--info {
  background:       var(--color-info-bg);
  border-color:     var(--color-info-subtle);
  border-left-color: var(--color-info);
  color:            var(--color-info-text);
}
.alert--success {
  background:       var(--color-success-bg);
  border-color:     var(--color-success-subtle);
  border-left-color: var(--color-success);
  color:            var(--color-success-text);
}
```

---

## Toast notification

Ephemeral. Appears in top-right corner. For async completions that
happened while the user was doing something else.

Rules: success/info auto-dismiss after 5s. Error/warning persist until dismissed.

```html
<!-- Toast container (fixed, top-right) -->
<div class="toast-stack" aria-live="polite" aria-atomic="false">

  <!-- Success (auto-dismiss) -->
  <div class="toast toast--success" role="status">
    <svg class="toast__icon" aria-hidden="true"><!-- check --></svg>
    <div class="toast__body">
      <p class="toast__title type-heading-group">Job complete</p>
      <p class="toast__desc type-body-xs text-secondary">bert-finetune-run-3 finished in 4h 31m.</p>
    </div>
    <button class="toast__dismiss btn btn--outlined btn--sm btn--icon-only"
            type="button" aria-label="Dismiss notification">
      <svg class="btn__icon" aria-hidden="true"><!-- x --></svg>
    </button>
  </div>

  <!-- Error (persistent) -->
  <div class="toast toast--danger" role="alert">
    <svg class="toast__icon" aria-hidden="true"><!-- x-circle --></svg>
    <div class="toast__body">
      <p class="toast__title type-heading-group">Submission failed</p>
      <p class="toast__desc type-body-xs text-secondary">
        No matching GPU available. Retry or adjust constraints.
      </p>
    </div>
    <div class="toast__actions">
      <button class="btn btn--outlined btn--sm" type="button">Retry</button>
      <button class="toast__dismiss btn btn--outlined btn--sm btn--icon-only"
              type="button" aria-label="Dismiss">
        <svg class="btn__icon" aria-hidden="true"><!-- x --></svg>
      </button>
    </div>
  </div>

</div>
```

```css
.toast-stack {
  position:      fixed;
  top:           calc(var(--topbar-height) + var(--space-4));
  right:         var(--space-5);
  z-index:       60;
  display:       flex;
  flex-direction: column;
  gap:           var(--space-2);
  max-width:     380px;
  width:         100%;
}
.toast {
  display:       flex;
  align-items:   flex-start;
  gap:           var(--space-3);
  padding:       var(--space-3) var(--space-4);
  background:    var(--color-surface-1);
  border:        1px solid var(--color-border-default);
  border-left-width: 3px;
  border-radius: var(--radius-md);
  box-shadow:    var(--shadow-md);
  animation:     toast-in var(--duration-slow) var(--ease-out);
}
@keyframes toast-in {
  from { transform: translateX(calc(100% + var(--space-5))); opacity: 0; }
  to   { transform: translateX(0); opacity: 1; }
}
.toast__icon  { flex-shrink: 0; width: 16px; height: 16px; margin-top: 1px; }
.toast__body  { flex: 1; display: flex; flex-direction: column; gap: 2px; }
.toast__title { margin: 0; }
.toast__desc  { margin: 0; }
.toast__actions { display: flex; align-items: center; gap: var(--space-2); flex-shrink: 0; }
.toast__dismiss { flex-shrink: 0; }

.toast--success { border-left-color: var(--color-success); }
.toast--danger  { border-left-color: var(--color-danger); }
.toast--warning { border-left-color: var(--color-warning); }
.toast--info    { border-left-color: var(--color-info); }
```

---

## Tooltip

Simple hover label for icon-only buttons and truncated content.

```html
<!-- Wrap the target element -->
<span class="tooltip-wrapper">
  <button class="btn btn--outlined btn--sm btn--icon-only"
          type="button" aria-label="Filter jobs"
          aria-describedby="tip-filter">
    <svg class="btn__icon" aria-hidden="true"><!-- filter --></svg>
  </button>
  <span class="tooltip" id="tip-filter" role="tooltip">Filter jobs</span>
</span>
```

```css
.tooltip-wrapper { position: relative; display: inline-flex; }
.tooltip {
  position:      absolute;
  bottom:        calc(100% + var(--space-1));
  left:          50%;
  transform:     translateX(-50%);
  padding:       var(--space-1) var(--space-2);
  background:    var(--color-surface-3);
  border:        1px solid var(--color-border-default);
  border-radius: var(--radius-sm);
  font-family:   var(--font-sans);
  font-size:     var(--text-12);
  color:         var(--color-text-primary);
  white-space:   nowrap;
  box-shadow:    var(--shadow-sm);
  pointer-events: none;
  opacity:       0;
  transition:    opacity var(--duration-fast) var(--ease-out);
  z-index:       50;
}
.tooltip-wrapper:hover .tooltip,
.tooltip-wrapper:focus-within .tooltip { opacity: 1; }
```

---

## Empty state

Every list, table, or feed must define what happens at zero items.
Empty states are not dead ends — they are onboarding moments.

```html
<!-- Job queue — empty -->
<div class="empty-state">
  <div class="empty-state__icon">
    <svg aria-hidden="true"><!-- inbox icon, 40×40 --></svg>
  </div>
  <h2 class="type-heading-section">No jobs running</h2>
  <p class="type-body-md text-secondary">
    Jobs will appear here once submitted. Your cluster is ready.
  </p>
  <a class="btn btn--primary" href="/jobs/new">Submit a job →</a>
</div>

<!-- Search results — zero matches -->
<div class="empty-state">
  <div class="empty-state__icon">
    <svg aria-hidden="true"><!-- search icon --></svg>
  </div>
  <h2 class="type-heading-section">No results for "a100-node-99"</h2>
  <p class="type-body-md text-secondary">
    Try a different node ID or check that the node is connected.
  </p>
  <button class="btn btn--secondary" type="button"
          onclick="clearSearch()">Clear search</button>
</div>

<!-- Filtered — no matches -->
<div class="empty-state">
  <h2 class="type-heading-section">No failed jobs</h2>
  <p class="type-body-md text-secondary">All jobs are running or queued.</p>
  <button class="btn btn--outlined btn--sm" type="button">Clear filters</button>
</div>
```

```css
.empty-state {
  display:        flex;
  flex-direction: column;
  align-items:    center;
  justify-content: center;
  text-align:     center;
  gap:            var(--space-4);
  padding:        var(--space-8) var(--space-5);
  min-height:     280px;
}
.empty-state__icon {
  width:       48px;
  height:      48px;
  color:       var(--color-text-tertiary);
  display:     flex;
  align-items: center;
  justify-content: center;
}
.empty-state h2 { margin: 0; }
.empty-state p  { margin: 0; max-width: 400px; }
```

---

## Rules

- **`role="alert"` for errors and warnings** — screen readers announce immediately.
- **`role="status"` for success and info** — announced politely (next pause).
- **`aria-live="polite"` on the toast container** — not on individual toasts.
- **Never celebrate routine actions.** Only show success for non-trivial completions (job finished, export ready). Not for saving a setting.
- **Error toasts persist.** Auto-dismiss is only for success and info.
- **Empty state always has three parts:** title (what's empty) + description (why + what it means) + action (what to do next). Never just "No data."
- **Tooltip content duplicates `aria-label`** on icon-only buttons — it's a visible supplement, not a replacement for the accessible name.
