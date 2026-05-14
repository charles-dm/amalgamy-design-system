# Component: Sidebar Navigation

## When to load this file
When building any LaunchHPC screen with persistent left navigation. This is the
primary wayfinding component — use the pre-built component, do not construct
ad-hoc nav lists. For topbar / tabs / breadcrumbs (other nav patterns) see
`nav-SKILL.md`.

## Figma source
Component sets: **SidebarNav/Collapsed** (56px) · **SidebarNav/Expanded** (300px) ·
**SidebarItem** (16 variants: Type × State × Expanded)
File: R5IJ7h4lSqJq6smVlswkLQ · Page: 🧩 Components

Live prototype: [`sidebar-nav.html`](sidebar-nav.html)

---

## Variants

### State (sidebar-level)
| Value | Width | Use |
|---|---|---|
| `expanded` | 300px | **Default.** Icon + label. Use on most screens. |
| `collapsed` | 56px | Icon-only. Horizontal space constrained, or user explicitly collapsed. |

### Type (item-level)
| Value | Use |
|---|---|
| `primary` | Top section — main routes (Fleet, Jobs, Policies, Reports, Audit). Max 5 items. |
| `bottom`  | Bottom section — utility routes (Settings, Account, Sign out). Pinned with `flex: 1 0 0` spacer above. |

### State (item-level)
| Value | Treatment |
|---|---|
| `default`  | Icon `--color-text-faint`, label `--color-text-secondary`, transparent bg |
| `hover`    | Icon + label brighten to `--color-text-primary`, bg `rgba(255,255,255,0.04)` |
| `active`   | Icon + label `--color-accent`, bg `--color-accent-muted-bg` (#082223), right indicator `box-shadow: inset -2px 0 0 0 var(--color-accent)` |
| `disabled` | Icon `--color-text-faint @ 0.4`, label `--color-text-faint`, no hover |

---

## HTML anatomy

### Sidebar shell (expanded — default)

```html
<aside class="sidebar" aria-label="Primary navigation">

  <!-- Logo box — high-emphasis, uses brand/teal/hover border -->
  <div class="sidebar__brand">
    <span class="sidebar__brand-mark"></span>
    <span class="sidebar__brand-name">AMALGAMY</span>
  </div>

  <!-- Primary nav items — max 5 -->
  <nav class="sidebar__nav">
    <a class="sidebar-item sidebar-item--active" href="/fleet"
       data-label="Fleet" aria-current="page">
      <i class="ph-bold ph-cpu sidebar-item__icon"></i>
      <span class="sidebar-item__label">Fleet</span>
    </a>
    <a class="sidebar-item" href="/jobs" data-label="Jobs">
      <i class="ph ph-list-bullets sidebar-item__icon"></i>
      <span class="sidebar-item__label">Jobs</span>
    </a>
    <a class="sidebar-item" href="/policies" data-label="Policies">
      <i class="ph ph-shield-check sidebar-item__icon"></i>
      <span class="sidebar-item__label">Policies</span>
    </a>
    <a class="sidebar-item" href="/reports" data-label="Reports">
      <i class="ph ph-chart-line sidebar-item__icon"></i>
      <span class="sidebar-item__label">Reports</span>
    </a>
    <a class="sidebar-item" href="/audit" data-label="Audit">
      <i class="ph ph-file-text sidebar-item__icon"></i>
      <span class="sidebar-item__label">Audit</span>
    </a>
  </nav>

  <!-- Spacer — REQUIRED flex: 1 0 0 (not flex: 1 alone) -->
  <div class="sidebar__spacer" aria-hidden="true"></div>

  <!-- Bottom items — pinned to bottom -->
  <nav class="sidebar__bottom" aria-label="Account">
    <a class="sidebar-item" href="/settings" data-label="Settings">
      <i class="ph ph-gear-six sidebar-item__icon"></i>
      <span class="sidebar-item__label">Settings</span>
    </a>
    <a class="sidebar-item" href="/account" data-label="Account">
      <i class="ph ph-user sidebar-item__icon"></i>
      <span class="sidebar-item__label">Account</span>
    </a>
  </nav>

</aside>
```

### Collapsed state

Toggle with a class on the parent `.app-shell`:

```html
<div class="app-shell app-shell--sidebar-collapsed">
  <aside class="sidebar"> ... </aside>
  <main class="main"> ... </main>
</div>
```

The sidebar reads the parent class — no markup changes inside `.sidebar` itself. CSS hides labels, the brand name, and reduces width.

---

## CSS

```css
/* Sidebar shell ─────────────────────────────────────────────── */
.sidebar {
  display:        flex;
  flex-direction: column;
  width:          var(--sidebar-width);          /* 300px (v0.3) */
  background:     var(--color-surface-1);
  border-right:   1px solid var(--color-border-default);
  overflow-y:     auto;
  overflow-x:     hidden;
  transition:     width 220ms cubic-bezier(0.4, 0, 0.2, 1);
}

.app-shell--sidebar-collapsed .sidebar {
  width: var(--sidebar-width-collapsed);          /* 56px (v0.3) */
}

/* Logo box — uses brand/teal/HOVER (#45D9CD), not default (#2ECEC0).
   Intentional: the logo box is high-emphasis. */
.sidebar__brand {
  display:       flex;
  align-items:   center;
  gap:           var(--space-3);
  height:        var(--topbar-height);            /* aligns with main topbar */
  padding:       0 var(--space-4);
  border-bottom: 1px solid var(--color-border-default);
  border-right:  none; /* the sidebar itself owns the right border */
}
.sidebar__brand-mark {
  width:        24px;
  height:       24px;
  border:       1px solid var(--color-accent-hover);   /* #45D9CD */
  flex-shrink:  0;
}
.sidebar__brand-name {
  font-family:    var(--font-mono);
  font-weight:    600;
  font-size:      14px;
  letter-spacing: 0.10em;
  color:          var(--color-text-primary);
}

/* Sections — no horizontal padding here, padding lives on each item */
.sidebar__nav,
.sidebar__bottom {
  display:        flex;
  flex-direction: column;
  padding:        var(--space-2) 0;
}
.sidebar__bottom {
  border-top: 1px solid var(--color-border-subtle);
}

/* Spacer — flex: 1 0 0 prevents collapse when content is tall.
   flex: 1 alone uses default flex-shrink: 1 which can collapse the spacer. */
.sidebar__spacer {
  flex:       1 0 0;
  min-height: 1px;
}

/* SidebarItem ───────────────────────────────────────────────── */
.sidebar-item {
  position:        relative;
  display:         flex;
  align-items:     center;
  gap:             12px;
  height:          44px;
  padding:         0 16px;
  text-decoration: none;
  white-space:     nowrap;
  overflow:        hidden;
  cursor:          pointer;
  transition:      background 120ms ease-out,
                   color      120ms ease-out;
}

.sidebar-item__icon {
  width:       24px;       /* always 24x24, never resizes */
  height:      24px;
  font-size:   24px;       /* Phosphor: font-size controls glyph size */
  color:       var(--color-text-faint);
  flex-shrink: 0;
  transition:  color 120ms ease-out;
}

.sidebar-item__label {
  font-family:    var(--font-mono);
  font-weight:    var(--weight-medium);          /* 500 */
  font-size:      12px;
  text-transform: uppercase;
  letter-spacing: 1.2px;                          /* ~0.10em at 12px */
  color:          var(--color-text-secondary);
  flex:           1;
  overflow:       hidden;
  text-overflow:  ellipsis;
  transition:     opacity 160ms 30ms ease-out,
                  transform 160ms 30ms cubic-bezier(0.4, 0, 0.2, 1),
                  color 120ms ease-out;
}

/* Hover ─────────────────────────────────────────────────────── */
.sidebar-item:hover:not(.sidebar-item--active) {
  background: rgba(255, 255, 255, 0.04);
}
.sidebar-item:hover:not(.sidebar-item--active) .sidebar-item__icon,
.sidebar-item:hover:not(.sidebar-item--active) .sidebar-item__label {
  color: var(--color-text-primary);
}

/* Active — all signals fire at once.
   Right indicator is box-shadow inset, NOT border-right (which would shift content). */
.sidebar-item--active {
  background: var(--color-accent-muted-bg);                   /* #082223 */
  box-shadow: inset -2px 0 0 0 var(--color-accent);           /* #2ECEC0 */
}
.sidebar-item--active .sidebar-item__icon,
.sidebar-item--active .sidebar-item__label {
  color: var(--color-accent);
}

/* Disabled */
.sidebar-item--disabled {
  pointer-events: none;
  opacity:        0.4;
}

/* Collapsed state — icon-only ───────────────────────────────── */
.app-shell--sidebar-collapsed .sidebar-item {
  padding: 0;
  justify-content: center;
}
.app-shell--sidebar-collapsed .sidebar-item__label {
  opacity:   0;
  transform: translateX(-6px);
  width:     0;
  pointer-events: none;
}
.app-shell--sidebar-collapsed .sidebar__brand-name {
  display: none;
}
.app-shell--sidebar-collapsed .sidebar__brand {
  justify-content: center;
  padding: 0;
}

/* Collapsed tooltip — CSS-only via data-label attribute (no JS) */
.app-shell--sidebar-collapsed .sidebar-item::after {
  content:       attr(data-label);
  position:      absolute;
  left:          calc(100% + 8px);
  top:           50%;
  transform:     translateY(-50%);
  padding:       6px 10px;
  background:    var(--color-surface-3);
  border:        1px solid var(--color-border-default);
  color:         var(--color-text-primary);
  font-family:   var(--font-mono);
  font-size:     12px;
  letter-spacing: 0.04em;
  white-space:   nowrap;
  opacity:       0;
  pointer-events: none;
  transition:    opacity 120ms ease-out;
  z-index:       10;
}
.app-shell--sidebar-collapsed .sidebar-item:hover::after {
  opacity: 1;
}
```

---

## Conventions

- **Exactly one active item at all times.** Setting two `.sidebar-item--active`
  classes is a bug, not a design choice.
- **`aria-current="page"`** on the active anchor — pairs with the visual signal
  for screen readers.
- **Max 5 primary items.** Beyond that, group into a second section or move to
  a topbar tab. The sidebar is for top-level routes, not feature menus.
- **Labels come from route config, never hardcoded inline.** This is enforced
  at the build layer; the component just renders what it's given.
- **Always include `data-label="..."`** even on expanded items — the value powers
  the collapsed-state tooltip via CSS-only `::after { content: attr(data-label) }`.
- **Logo border uses `--color-accent-hover` (#45D9CD), not `--color-accent` (#2ECEC0).**
  The hover token is intentionally brighter for the logo's higher visual weight.
- **Spacer must be `flex: 1 0 0; min-height: 1px`** — not `flex: 1` alone.
  The explicit `flex-shrink: 0` prevents collapse when the nav content is tall.
- **No horizontal padding on `.sidebar__nav` or `.sidebar__bottom`.** Padding
  lives on each `.sidebar-item` (16px when expanded). Putting it on the container
  shifts the active indicator inward and breaks the right-edge alignment.

## Animation rules

- **Width-only transition.** Collapse: `width 220ms cubic-bezier(0.4, 0, 0.2, 1)`.
  Never animate `height`, `top`, `left`, or `transform: scaleX` — they cause
  reflow elsewhere or break the edge alignment.
- **Label reveal on expand.** Combine `opacity 0→1` with `transform: translateX(-6px → 0)`,
  duration 160ms, **delay 30ms** so labels appear after the sidebar has begun widening.
- **Icon does not animate.** It's a fixed 24×24 anchor; only the label moves around it.

---

## Common mistakes

| Mistake | Fix |
|---|---|
| `border-right: 2px solid var(--color-accent)` for active indicator | Use `box-shadow: inset -2px 0 0 0 var(--color-accent)` so content doesn't shift |
| Two items have `.sidebar-item--active` | Audit your active-state logic — only one path can be current |
| Active item missing `aria-current="page"` | Add it — visual + aria must agree |
| Padding on `.sidebar__nav` instead of `.sidebar-item` | Move padding onto each item |
| Spacer is `flex: 1` | Change to `flex: 1 0 0; min-height: 1px` |
| Collapsed sidebar has no tooltip | Add `data-label="..."` to each item; tooltip is CSS-only |
| Sidebar collapse animates with `transform: scaleX` | Animate `width` instead |
| Logo box uses `--color-accent` border | Use `--color-accent-hover` — intentionally brighter |
| Sidebar has 8 items | Max 5 in the primary section; group into a second section or move to topbar tabs |
| Mono labels missing | Labels are Geist Mono Medium 12px, uppercase, letter-spacing 1.2px — not sans |

---

## Migration note (from legacy `.nav-item`)

The legacy `.nav-item` component in `amalgamy-layout.css` predates this spec. New
work should use `.sidebar-item` per this skill. Differences:

| Property | `.nav-item` (legacy) | `.sidebar-item` (v0.3) |
|---|---|---|
| Active indicator | background + bold weight | box-shadow inset right indicator |
| Active bg | `--color-accent-subtle` | `--color-accent-muted-bg` (#082223) |
| Icon size | 20×20 | 24×24 |
| Label font | sans | **mono** |
| Tooltip when collapsed | none | CSS-only via `data-label` |

`.nav-item` will be deprecated in a future minor release; do not introduce new
references to it.
