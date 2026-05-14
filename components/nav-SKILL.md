# Component: Navigation

## When to load this file
When building the app shell — sidebar, topbar, tabs, breadcrumbs, or
any screen that requires wayfinding.

## Figma source
Component sets: **Sidebar Nav Item** (8) · **Tab (Bottom Border)** (8) · **Tab (Top Border)** (8) · **Breadcrumb Segment** (3) · **Accordion** (12)
File: R5IJ7h4lSqJq6smVlswkLQ · Page: 🧩 Components

---

## Sidebar navigation

### Anatomy

```html
<nav class="sidebar" aria-label="Main navigation">

  <!-- Logo / wordmark -->
  <div class="sidebar__logo">
    <img src="/logo.svg" alt="Amalgamy" width="120" height="24">
  </div>

  <!-- Nav section with label -->
  <div class="sidebar__section">
    <p class="sidebar__section-label type-ui-label">Operator</p>
    <ul class="sidebar__list" role="list">
      <li>
        <a class="nav-item nav-item--active" href="/fleet" aria-current="page">
          <svg class="nav-item__icon" aria-hidden="true"><!-- grid icon --></svg>
          <span class="nav-item__label">Fleet overview</span>
        </a>
      </li>
      <li>
        <a class="nav-item" href="/jobs">
          <svg class="nav-item__icon" aria-hidden="true"><!-- list icon --></svg>
          <span class="nav-item__label">Job queue</span>
          <span class="nav-item__count badge badge--neutral" aria-label="148 jobs">148</span>
        </a>
      </li>
      <li>
        <a class="nav-item" href="/alerts">
          <svg class="nav-item__icon" aria-hidden="true"><!-- bell icon --></svg>
          <span class="nav-item__label">Alerts</span>
          <span class="nav-item__count badge badge--danger" aria-label="3 alerts">3</span>
        </a>
      </li>
      <li>
        <a class="nav-item" href="/tenants">
          <svg class="nav-item__icon" aria-hidden="true"><!-- users icon --></svg>
          <span class="nav-item__label">Tenants</span>
        </a>
      </li>
    </ul>
  </div>

  <div class="sidebar__section">
    <p class="sidebar__section-label type-ui-label">Settings</p>
    <ul class="sidebar__list" role="list">
      <li>
        <a class="nav-item" href="/policies">
          <svg class="nav-item__icon" aria-hidden="true"><!-- shield icon --></svg>
          <span class="nav-item__label">Policies</span>
        </a>
      </li>
    </ul>
  </div>

  <!-- User / bottom -->
  <div class="sidebar__footer">
    <button class="nav-item nav-item--user" type="button"
            aria-label="User menu for c.hamilton@designmap.com">
      <span class="nav-item__avatar">CH</span>
      <span class="nav-item__label">C. Hamilton</span>
    </button>
  </div>
</nav>
```

### CSS

```css
.sidebar {
  grid-area:      sidebar;
  width:          var(--sidebar-width-expanded);
  height:         100vh;
  background:     var(--color-surface-1);
  border-right:   1px solid var(--color-border-subtle);
  display:        flex;
  flex-direction: column;
  overflow-y:     auto;
  overflow-x:     hidden;
  position:       fixed;
  top:            0;
  left:           0;
}

.sidebar__logo {
  padding: var(--space-4) var(--space-4);
  height:  var(--topbar-height);
  display: flex;
  align-items: center;
  border-bottom: 1px solid var(--color-border-subtle);
  flex-shrink: 0;
}

.sidebar__section {
  padding: var(--space-4) 0;
  border-bottom: 1px solid var(--color-border-subtle);
}

.sidebar__section-label {
  padding: 0 var(--space-4) var(--space-2);
  margin:  0;
  display: block;
}

.sidebar__list {
  list-style: none;
  margin: 0; padding: 0;
}

.sidebar__footer {
  margin-top: auto;
  padding: var(--space-3) 0;
  border-top: 1px solid var(--color-border-subtle);
}

/* Nav item */
.nav-item {
  display:     flex;
  align-items: center;
  gap:         var(--space-3);
  padding:     0 var(--space-4);
  height:      30px;
  width:       100%;
  font-family: var(--font-sans);
  font-size:   var(--text-14);
  font-weight: var(--weight-regular);
  color:       var(--color-text-secondary);
  text-decoration: none;
  border-radius: 0;
  transition:  background var(--duration-fast) var(--ease-out),
               color      var(--duration-fast) var(--ease-out);
  cursor:      pointer;
  border:      none;
  background:  none;
}
.nav-item:hover {
  background: var(--color-surface-2);
  color:      var(--color-text-primary);
}
.nav-item--active,
.nav-item[aria-current="page"] {
  background: var(--color-accent-muted-bg);
  color:      var(--color-accent);
  font-weight: var(--weight-medium);
  position: relative;
}
.nav-item--active::before,
.nav-item[aria-current="page"]::before {
  content:  '';
  position: absolute;
  left: 0; top: 0; bottom: 0;
  width: 2px;
  background: var(--color-accent);
  border-radius: 0 1px 1px 0;
}
.nav-item__icon {
  width:       16px;
  height:      16px;
  flex-shrink: 0;
  color:       inherit;
}
.nav-item__label { flex: 1; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.nav-item__count { flex-shrink: 0; }

/* Avatar */
.nav-item__avatar {
  width:           28px;
  height:          28px;
  border-radius:   var(--radius-full);
  background:      var(--color-accent-muted-bg);
  color:           var(--color-accent);
  font-family:     var(--font-mono);
  font-size:       var(--text-11);
  font-weight:     var(--weight-medium);
  display:         flex;
  align-items:     center;
  justify-content: center;
  flex-shrink:     0;
}

/* Collapsed sidebar */
.app-shell--collapsed .sidebar { width: var(--sidebar-width-collapsed); }
.app-shell--collapsed .sidebar__section-label,
.app-shell--collapsed .nav-item__label,
.app-shell--collapsed .nav-item__count { display: none; }
.app-shell--collapsed .nav-item { justify-content: center; padding: 0; }
.app-shell--collapsed .nav-item__icon { width: 20px; height: 20px; }
```

---

## Breadcrumb

> Figma: BreadcrumbItem (3322:1749) · Breadcrumb/Separator (3322:1750)
> 8 variants + 5 assembled example trails

### BreadcrumbItem variants

| Type | State | Color | Treatment |
|---|---|---|---|
| `link` | `default` | text/tertiary #8CADA7 | no underline |
| `link` | `hover` | brand/teal #2ECEC0 | underline |
| `link` | `active` | teal/subtle #1F908A | underline |
| `link` | `disabled` | text/faint #78948F at 40% opacity | no underline |
| `current` | `default` | text/primary #EAF1EC | no underline, non-interactive |
| `current` | `hover` | text/primary #EAF1EC | no change (current is static) |
| `current` | `disabled` | text/faint #78948F at 40% opacity | — |

**Text style:** `body/sm` — Geist Regular 14px, 150% line-height.
All text nodes bound to color variables via `componentPropertyReferences`.

### Separator

Forward slash `/` — not a chevron. Vector path: `M 10 16 L 6 4`.
Stroke: `text/faint` (#78948F), stroke-weight: 1px, round cap.
Frame size: 16×20px.

### HTML scaffold

```html
<!-- 3-level breadcrumb trail -->
<nav aria-label="Breadcrumb">
  <ol class="breadcrumb" role="list">
    <li>
      <a class="breadcrumb__item breadcrumb__item--link" href="/workflows">
        Workflows
      </a>
    </li>
    <li class="breadcrumb__sep" aria-hidden="true">/</li>
    <li>
      <a class="breadcrumb__item breadcrumb__item--link" href="/workflows/test-run-2">
        Test Run 2
      </a>
    </li>
    <li class="breadcrumb__sep" aria-hidden="true">/</li>
    <li>
      <span class="breadcrumb__item breadcrumb__item--current" aria-current="page">
        Monitor
      </span>
    </li>
  </ol>
</nav>
```

### CSS

```css
.breadcrumb {
  display: flex;
  align-items: center;
  gap: 4px;
  list-style: none;
  padding: 0; margin: 0;
}
.breadcrumb__item--link {
  font: 400 14px/1.5 var(--font-sans);
  color: var(--color-text-tertiary);
  text-decoration: none;
  padding: 2px 0;
  transition: color 120ms;
}
.breadcrumb__item--link:hover {
  color: var(--color-accent);
  text-decoration: underline;
}
.breadcrumb__item--current {
  font: 400 14px/1.5 var(--font-sans);
  color: var(--color-text-primary);
}
.breadcrumb__sep {
  font: 400 14px/1 var(--font-sans);
  color: var(--color-text-faint);
  user-select: none;
}
```

### Rules

- The last item is always `Type=current` — never a link
- Never use a chevron as separator — always a forward slash
- Always wrap in `<nav aria-label="Breadcrumb">` with `<ol role="list">`
- Separator items get `aria-hidden="true"`
- Current item gets `aria-current="page"`
- Maximum 4 levels before truncating with `…` — deeper trails confuse

---

## Topbar

```html
<header class="topbar" role="banner">
  <div class="topbar__left">
    <!-- Sidebar toggle -->
    <button class="btn btn--outlined btn--sm btn--icon-only"
            type="button" aria-label="Toggle sidebar"
            aria-expanded="true">
      <svg class="btn__icon" aria-hidden="true"><!-- menu icon --></svg>
    </button>
    <!-- Breadcrumb -->
    <nav class="breadcrumb" aria-label="Breadcrumb">
      <ol class="breadcrumb__list">
        <li class="breadcrumb__item">
          <a class="breadcrumb__link" href="/fleet">Fleet</a>
        </li>
        <li class="breadcrumb__sep" aria-hidden="true">/</li>
        <li class="breadcrumb__item">
          <span class="breadcrumb__current" aria-current="page">a100-node-07</span>
        </li>
      </ol>
    </nav>
  </div>
  <div class="topbar__right">
    <!-- Global search -->
    <div class="field field--search topbar__search">
      <svg class="field__search-icon" aria-hidden="true"><!-- search --></svg>
      <input class="input input--sm input--search" type="search"
             placeholder="Search..." aria-label="Global search">
    </div>
    <!-- Notifications -->
    <button class="btn btn--outlined btn--sm btn--icon-only topbar__notif"
            type="button" aria-label="3 alerts">
      <svg class="btn__icon" aria-hidden="true"><!-- bell --></svg>
      <span class="topbar__notif-badge" aria-hidden="true">3</span>
    </button>
  </div>
</header>
```

```css
.topbar {
  grid-area:   topbar;
  height:      var(--topbar-height);
  background:  var(--color-surface-1);
  border-bottom: 1px solid var(--color-border-subtle);
  display:     flex;
  align-items: center;
  justify-content: space-between;
  padding:     0 var(--space-4);
  position:    sticky;
  top:         0;
  z-index:     20;
}
.topbar__left, .topbar__right {
  display:     flex;
  align-items: center;
  gap:         var(--space-3);
}
.topbar__search { width: 240px; }
.topbar__notif  { position: relative; }
.topbar__notif-badge {
  position:    absolute;
  top:         -4px;
  right:       -4px;
  width:       16px;
  height:      16px;
  background:  var(--color-danger);
  color:       #fff;
  font-family: var(--font-mono);
  font-size:   var(--text-9);
  font-weight: var(--weight-bold);
  border-radius: var(--radius-full);
  display:     flex;
  align-items: center;
  justify-content: center;
}

/* Breadcrumb */
.breadcrumb__list { display: flex; align-items: center; gap: var(--space-2); list-style: none; margin: 0; padding: 0; }
.breadcrumb__link { font-size: var(--text-12); color: var(--color-text-secondary); text-decoration: none; }
.breadcrumb__link:hover { color: var(--color-text-primary); }
.breadcrumb__sep  { font-size: var(--text-12); color: var(--color-text-tertiary); }
.breadcrumb__current { font-size: var(--text-12); color: var(--color-text-primary); font-weight: var(--weight-medium); }
```

---

## Tabs

Use when a single page has 2–5 distinct views of the same subject.

```html
<!-- Bottom border variant (most common — content pages) -->
<div class="tabs" role="tablist" aria-label="Node detail">
  <button class="tab tab--active" role="tab"
          aria-selected="true" aria-controls="panel-overview" id="tab-overview">
    Overview
  </button>
  <button class="tab" role="tab"
          aria-selected="false" aria-controls="panel-jobs" id="tab-jobs">
    Jobs
    <span class="tab__count">12</span>
  </button>
  <button class="tab" role="tab"
          aria-selected="false" aria-controls="panel-ledger" id="tab-ledger">
    Decision Ledger
  </button>
  <button class="tab" role="tab" aria-selected="false" disabled>
    Metrics
  </button>
</div>

<!-- Tab panels -->
<div id="panel-overview" role="tabpanel" aria-labelledby="tab-overview">
  <!-- content -->
</div>
<div id="panel-jobs" role="tabpanel" aria-labelledby="tab-jobs" hidden>
  <!-- content -->
</div>
```

```css
.tabs {
  display:       flex;
  gap:           0;
  border-bottom: 1px solid var(--color-border-subtle);
  margin-bottom: var(--space-5);
}
.tab {
  display:     inline-flex;
  align-items: center;
  gap:         var(--space-2);
  padding:     0 var(--space-4);
  height:      40px;
  font-family: var(--font-sans);
  font-size:   var(--text-14);
  font-weight: var(--weight-regular);
  color:       var(--color-text-secondary);
  background:  none;
  border:      none;
  border-bottom: 2px solid transparent;
  cursor:      pointer;
  white-space: nowrap;
  transition:  color var(--duration-base) var(--ease-out),
               border-color var(--duration-base) var(--ease-out);
  margin-bottom: -1px; /* overlap container border */
}
.tab:hover    { color: var(--color-text-primary); }
.tab--active,
.tab[aria-selected="true"] {
  color:        var(--color-text-primary);
  font-weight:  var(--weight-medium);
  border-bottom-color: var(--color-accent);
}
.tab:disabled { opacity: 0.4; cursor: not-allowed; }
.tab__count   {
  font-family: var(--font-mono);
  font-size:   var(--text-11);
  color:       inherit;
  opacity:     0.7;
}
```

---

## Rules

- **Exactly one active nav item at all times.** Zero = user is lost. Two = user is confused.
- **`aria-current="page"` on the active sidebar link** — not a class alone.
- **`aria-selected` on tabs** — managed in JS when tab changes.
- **Hidden tab panels use `hidden` attribute** — not `display: none` in CSS (the attribute is semantic).
- **Breadcrumb uses `<nav>` + `<ol>`.** It's an ordered list of locations.
- **Collapsed sidebar shows icons only** — always add tooltip on hover via `title` or a custom tooltip component.
