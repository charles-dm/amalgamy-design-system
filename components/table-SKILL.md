# Component: Table

## When to load this file
When building any screen that displays a list of comparable items —
operator fleet view, job queue, tenant list, alert log, audit entries.

## Figma source
Density system is defined via text styles — table/caps-lg/md/sm and table/cell-lg/md/sm.
No dedicated Table component set in Figma — tables are assembled from density tokens.
File: R5IJ7h4lSqJq6smVlswkLQ · Page: 🧩 Components

---

## Density system

Choose ONE density per table. Never mix row heights.

| Density | Row height | Cap style | Cell style | When |
|---|---|---|---|---|
| `lg` | 44px | `table/caps-lg` 11px | `table/cell-lg` 13px | Operator fleet overview, primary table |
| `md` | 36px | `table/caps-md` 10px | `table/cell-md` 11px | Most operator tables — default |
| `sm` | 28px | `table/caps-sm` 9px | `table/cell-sm` 10px | Audit logs, Decision Ledger, event feeds |

---

## HTML anatomy

### Medium density (default)
```html
<div class="table-container">
  <table class="table table--md">
    <thead>
      <tr>
        <th class="col-node">Node</th>
        <th class="col-status">Status</th>
        <th class="col-util" data-align="right">GPU Util</th>
        <th class="col-jobs" data-align="right">Jobs</th>
        <th class="col-actions" data-align="center">
          <span class="sr-only">Actions</span>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr class="table-row">
        <td class="col-node">
          <span class="data-value">a100-node-07</span>
        </td>
        <td class="col-status">
          <span class="badge badge--success">
            <span class="status-dot status-dot--success" aria-hidden="true"></span>
            Running
          </span>
        </td>
        <td class="col-util" data-align="right">
          <span class="data-value">94.2%</span>
        </td>
        <td class="col-jobs" data-align="right">
          <span class="data-value">12</span>
        </td>
        <td class="col-actions" data-align="center">
          <button class="btn btn--outlined btn--sm btn--icon-only" aria-label="View node a100-node-07">
            <svg class="btn__icon" aria-hidden="true"><!-- arrow-right --></svg>
          </button>
        </td>
      </tr>
      <!-- Hover state — CSS handles this -->
      <!-- Selected row -->
      <tr class="table-row table-row--selected">
        <td class="col-node">
          <span class="data-value">a100-node-09</span>
        </td>
        <!-- ... -->
      </tr>
    </tbody>
  </table>
</div>
```

### Large density
```html
<table class="table table--lg"> ... </table>
```

### Small density (audit log)
```html
<table class="table table--sm"> ... </table>
```

### Sortable column header
```html
<th class="col-util col--sortable col--sorted-desc" data-align="right"
    aria-sort="descending">
  <button class="th-sort" type="button">
    GPU Util
    <svg class="th-sort__icon" aria-hidden="true"><!-- chevron-down --></svg>
  </button>
</th>
```

### Empty state (required)
```html
<tbody>
  <tr>
    <td colspan="5" class="table-empty">
      <div class="table-empty__inner">
        <p class="type-heading-group">No nodes found</p>
        <p class="type-body-sm text-secondary">
          Nodes appear here once connected to the cluster.
        </p>
        <a href="/setup" class="btn btn--outlined btn--sm">Set up cluster →</a>
      </div>
    </td>
  </tr>
</tbody>
```

---

## CSS

```css
/* Container */
.table-container {
  overflow-x: auto;
  border:     1px solid var(--color-border-subtle);
  border-radius: var(--radius-lg);
}

/* Base table */
.table {
  width:           100%;
  border-collapse: collapse;
}

/* Headers */
.table th {
  font-family:    var(--font-mono);
  font-weight:    var(--weight-bold);
  text-transform: uppercase;
  color:          var(--color-text-secondary);
  text-align:     left;
  background:     var(--color-surface-2);
  border-bottom:  1px solid var(--color-border-default);
  white-space:    nowrap;
}
.table th[data-align="right"] { text-align: right; }
.table th[data-align="center"] { text-align: center; }

/* Density — headers */
.table--lg th { font-size: var(--text-11); letter-spacing: var(--tracking-caps-lg); padding: 0 var(--space-4); height: 44px; }
.table--md th { font-size: var(--text-10); letter-spacing: var(--tracking-caps-md); padding: 0 var(--space-4); height: 36px; }
.table--sm th { font-size: var(--text-9);  letter-spacing: var(--tracking-caps-md); padding: 0 var(--space-3); height: 28px; }

/* Cells */
.table td {
  font-family:          var(--font-mono);
  color:                var(--color-text-primary);
  font-variant-numeric: tabular-nums;
  border-bottom:        1px solid var(--color-border-subtle);
  vertical-align:       middle;
}
.table td[data-align="right"]  { text-align: right; }
.table td[data-align="center"] { text-align: center; }
.table tbody tr:last-child td  { border-bottom: none; }

/* Density — cells */
.table--lg td { font-size: var(--text-13); line-height: var(--leading-138); padding: 0 var(--space-4); height: 44px; }
.table--md td { font-size: var(--text-11); line-height: var(--leading-145); padding: 0 var(--space-4); height: 36px; }
.table--sm td { font-size: var(--text-10); line-height: var(--leading-140); padding: 0 var(--space-3); height: 28px; }

/* Row states */
.table-row { transition: background var(--duration-fast) var(--ease-out); }
.table-row:hover { background: var(--color-surface-2); }
.table-row--selected { background: var(--color-accent-muted-bg); }
.table-row--selected td { border-bottom-color: var(--color-accent-subtle); }

/* Sortable column */
.col--sortable .th-sort {
  display:     inline-flex;
  align-items: center;
  gap:         var(--space-1);
  background:  none;
  border:      none;
  font:        inherit;
  color:       inherit;
  cursor:      pointer;
  padding:     0;
  letter-spacing: inherit;
  text-transform: inherit;
}
.col--sortable:hover th { color: var(--color-text-primary); }
.col--sorted-asc .th-sort__icon,
.col--sorted-desc .th-sort__icon { color: var(--color-accent); }
.th-sort__icon { width: 12px; height: 12px; }

/* Empty state */
.table-empty { text-align: center; padding: var(--space-7) var(--space-5); }
.table-empty__inner {
  display:        flex;
  flex-direction: column;
  align-items:    center;
  gap:            var(--space-3);
  max-width:      400px;
  margin:         0 auto;
}
```

---

## Column alignment rule
- Text/ID columns: `text-align: left`
- Number/metric columns: `text-align: right` (use `data-align="right"`)
- Status/action columns: `text-align: center` (use `data-align="center"`)
- Headers always match their column's data alignment

## Rules

- **One density per table.** Never use `table--lg` headers with `table--md` cells.
- **Always define an empty state.** No blank `<tbody>` — ever.
- **Data values use `.data-value`** (Geist Mono, tabular-nums) inside `<td>`.
- **Status cells use `.badge`** with a `.status-dot` — never color alone.
- **Sortable columns need `aria-sort`** on the `<th>` (`ascending`, `descending`, or `none`).
- **Actions column last.** Never put action buttons in the first column.
- **Sticky header for long tables.** Add `position: sticky; top: 0; z-index: 1;` to `thead`.
