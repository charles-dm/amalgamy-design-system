---
name: amalgamy-navigation
description: Navigation patterns for Amalgamy LaunchHPC prototypes — horizontal tab sets (Fleet · Jobs · Policies), breadcrumbs (launchhpc / cluster-a / fleet), vertical sidebars (full app shell), and sub-navs (filter toggles, view switchers, segmented controls inside a section). Use this skill whenever the user asks for "tabs", "navigation", "sidebar", "breadcrumb", "filter bar", "view toggle", or asks to build a full screen with persistent navigation chrome. Depends on the amalgamy-sovereign-terminal foundation skill — load that first.
---

# Navigation

Four patterns, one family. Pick the right one for the screen, don't compose all four on the same view.

## When to use what

| Pattern | When | Example |
|---------|------|---------|
| **Horizontal tabs** | Switching between sibling views inside a section. The most common nav pattern. | `Fleet · Jobs · Policies · Reports · Audit` |
| **Breadcrumb** | Showing where the user is in a deep hierarchy. Always lives in the status bar. | `launchhpc / cluster-a / fleet` |
| **Vertical sidebar** | Full app shells — multi-section navigation persistent across many screens. | The index page; a settings panel with 8+ sections |
| **Sub-nav** | Filtering or toggling views *within* a single section. Subordinate to whatever nav got you there. | `All · Running · Queued · Failed` filters above a job list |

Two rules of thumb:

1. **Most prototypes use exactly one of the first three.** Tabs *or* breadcrumb-deep *or* sidebar — not all of them. A screen with a sidebar usually doesn't need top tabs; the sidebar is the top-level nav.
2. **Sub-nav layers on top of any of the others.** A sidebar-driven screen might still have a sub-nav for filtering. Tabs at the top, filters within a tab, etc.

## 1. Horizontal Tabs

The default for switching between sibling views. Hard-edged, monospace, aqua-active.

### Structure

```html
<nav class="tabs">
  <a class="tab active" href="#">fleet <span class="tcount">48</span></a>
  <a class="tab" href="#">jobs <span class="tcount">2,184</span></a>
  <a class="tab" href="#">policies <span class="tcount">37</span></a>
  <a class="tab" href="#">reports</a>
  <a class="tab" href="#">audit</a>
</nav>
```

With optional numbered prefixes (when the user provides them — see conventions):

```html
<nav class="tabs">
  <a class="tab active" href="#"><span class="tnum">1</span>fleet <span class="tcount">48</span></a>
  <a class="tab" href="#"><span class="tnum">2</span>jobs <span class="tcount">2,184</span></a>
  <a class="tab" href="#"><span class="tnum">3</span>policies <span class="tcount">37</span></a>
</nav>
```

### Token bindings

```css
.tabs {
  display: inline-flex;
  align-items: stretch;
  border-bottom: 1px solid var(--color-border-strong);
  /* No outer container border, no fill — tabs sit directly on the page */
}

.tab {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 10px 18px;
  font-family: var(--font-family-mono);
  font-size: var(--font-size-13);
  font-weight: var(--font-weight-regular);
  letter-spacing: var(--letter-spacing-normal);
  /* Lowercase, NOT uppercase — tabs are page labels, not buttons */
  color: var(--color-text-secondary);
  text-decoration: none;
  border-right: 1px solid var(--color-border-strong);
  /* 2px space reserved for the active top indicator so layout doesn't shift */
  border-top: 2px solid transparent;
  background: transparent;
}
.tab:last-child { border-right: none; }

.tab:hover {
  color: var(--color-text-primary);
}

.tab.active {
  color: var(--color-text-primary);
  font-weight: var(--font-weight-medium);
  border-top-color: var(--color-accent);
  /* Tinted teal-deep wash to lift the active cell off the page */
  background: color-mix(in srgb, var(--color-accent-subtle) 10%, transparent);
}

/* Optional leading number — dim, mono, position-marker */
.tab .tnum {
  color: var(--color-text-tertiary);
  font-size: var(--font-size-12);
  font-variant-numeric: tabular-nums;
}
.tab.active .tnum { color: var(--color-accent-subtle); }

/* Trailing count chip — meaningful counts only */
.tab .tcount {
  font-size: var(--font-size-11);
  color: var(--color-text-tertiary);
  font-variant-numeric: tabular-nums;
  /* No background, no border — the count is meta, not a chip */
}
.tab.active .tcount { color: var(--color-accent); }
```

### Conventions

- **The active signal is a 2px top indicator + a tinted background.** A 2px `--color-accent` (teal) line at the top of the active cell, plus a 10% `--color-accent-subtle` (teal-deep) wash filling the cell, plus full text-primary color, plus a step up to `font-weight-medium`. The active tab reads as a *lit panel* — the top indicator is the activation light, the wash is the warmth. Inactive tabs have a 2px transparent top reserve so layout doesn't shift on selection. Tabs are *page positions*, not buttons — this treatment looks deliberate and console-like, not like a button hover.
- **Lowercase labels, regular weight.** Tabs are page labels, not button text. Skip `text-transform: uppercase` and the `role-button` styling. Reach for `--font-size-13` body-sized text in `--font-weight-regular`. The visual quietness is what tells the user "these aren't buttons."
- **Optional leading numbers.** When the user provides numbered tabs (or the screen sequence has clear ordering — onboarding steps, a setup wizard), the leading `1` `2` `3` reinforces position-in-sequence semantics. The number is mono, dim (tertiary text), and at the same size as the label. Active state lifts the number to `--color-accent-subtle`. **Do not invent numbers** if the user doesn't provide them — for arbitrary sibling pages (Fleet · Jobs · Policies), numbers feel forced. They earn their place when the order is meaningful.
- **Trailing count chips are bare numbers.** Skip the bordered-pill background — the count is meta, not a chip. Just the number in tertiary text, with `tabular-nums`. On the active tab, the count lifts to `--color-accent` (teal) — a small piece of brand presence to anchor the active position.
- **Never use aqua on tabs.** Aqua (`--color-accent-secondary`) is reserved for primary CTAs only. Tabs are navigation, not action.
- **Bottom border on the container holds the row together visually.** Without it, inactive tabs float without a baseline. The container's `border-bottom: 1px solid var(--color-border-strong)` is the floor that anchors the row; vertical 1px separators between tabs (`border-right: 1px solid var(--color-border-strong)`) cell the row out. The active cell's 2px top indicator plus tinted fill announces the selection independently of the floor — no overlap trickery needed.

### Full-width tab variant

When tabs span the full width of a content panel (often above a primary work area):

```css
.tabs.full-width {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(0, 1fr));
}
.tabs.full-width .tab {
  justify-content: center;
}
```

Use this sparingly. The default inline tabs read as more "console" — full-width tabs read as more "app shell."

## 2. Breadcrumb

Lives **inside the status bar** at the top of full-screen views. Documented in the foundation skill's "Status bar" pattern but worth restating here because it's nav.

### Structure

```html
<span class="path">
  launchhpc <span class="sep">/</span>
  cluster-a <span class="sep">/</span>
  <span class="active">fleet</span>
</span>
```

### Token bindings

```css
.path {
  color: var(--color-text-secondary);
  font-size: var(--font-size-11);
}
.path .sep { color: var(--color-text-tertiary); }
.path .active { color: var(--color-text-primary); }
```

### Conventions

- **Lowercase, monospace, slash-separated.** This isn't a URL bar styled like one — it *is* a path, conceptually. Treat it as one.
- **The current location is in `--color-text-primary`** (brightest), parents are in `--color-text-secondary`, and separators are in `--color-text-tertiary`. The brightness rises as you read left-to-right toward "where you are."
- **No icons, no chevrons, no rounded backgrounds.** A `/` separator is the entire visual vocabulary.
- **Three segments is typical, four is the cap.** Deeper than that → use a sidebar instead, or compress the middle: `launchhpc / … / fleet`.
- **Segments are clickable** (each parent navigates up one level), but they look like text — the click affordance is implicit, not visual. Hover state may lift to `--color-text-primary` but no underline.

## 3. Vertical Sidebar

The full-app-shell pattern. Used when the prototype has 5+ destinations that need to be visible at all times.

### Structure

```html
<aside class="nav">
  <div class="nav-mark">
    <div class="row"><span class="dot"></span>AMALGAMY · LAUNCHHPC</div>
    <div class="sub">cluster-a · console</div>
  </div>

  <div class="nav-section">Operations</div>
  <a href="#" class="nav-link active"><span class="num">[01]</span>Fleet</a>
  <a href="#" class="nav-link"><span class="num">[02]</span>Jobs</a>
  <a href="#" class="nav-link"><span class="num">[03]</span>Queue</a>

  <div class="nav-section">Governance</div>
  <a href="#" class="nav-link"><span class="num">[04]</span>Policies</a>
  <a href="#" class="nav-link"><span class="num">[05]</span>Tenants</a>
  <a href="#" class="nav-link"><span class="num">[06]</span>Audit</a>

  <div class="nav-section">System</div>
  <a href="#" class="nav-link"><span class="num">[07]</span>Reports</a>
  <a href="#" class="nav-link"><span class="num">[08]</span>Settings</a>
</aside>
```

### Token bindings

```css
.nav {
  width: 240px;
  background: var(--color-surface-1);
  border-right: 1px solid var(--color-border-strong);
  padding: 32px 0;
  position: sticky;
  top: 0;
  height: 100vh;
  overflow-y: auto;
}

.nav-mark {
  padding: 0 24px 24px;
  border-bottom: 1px solid var(--color-border-strong);
  margin-bottom: 16px;
}
.nav-mark .row {
  display: flex; align-items: center; gap: 10px;
  font-family: var(--font-family-mono);
  font-size: var(--font-size-11);
  font-weight: var(--font-weight-semibold);
  letter-spacing: var(--letter-spacing-wide-1);
  color: var(--color-text-primary);
}
.nav-mark .row .dot {
  width: 8px; height: 8px;
  background: var(--color-accent);
  box-shadow: 0 0 0 2px rgba(46, 206, 192, 0.18);
}
.nav-mark .sub {
  margin-top: 8px;
  font-size: var(--font-size-10);
  color: var(--color-text-tertiary);
  letter-spacing: var(--letter-spacing-wide-1);
  text-transform: uppercase;
}

.nav-section {
  padding: 16px 24px 6px;
  font-size: var(--font-size-9);
  color: var(--color-text-tertiary);
  letter-spacing: var(--letter-spacing-caps);
  text-transform: uppercase;
  font-weight: var(--font-weight-semibold);
}

.nav-link {
  display: flex; align-items: center; gap: 10px;
  padding: 7px 24px;
  color: var(--color-text-secondary);
  font-size: var(--font-size-12);
  text-decoration: none;
  border-left: 2px solid transparent;
}
.nav-link:hover {
  color: var(--color-text-primary);
  background: var(--color-surface-2);
}
.nav-link.active {
  color: var(--color-accent);
  border-left-color: var(--color-accent);
  background: var(--color-surface-2);
}
.nav-link .num {
  color: var(--color-text-tertiary);
  font-size: var(--font-size-10);
  letter-spacing: var(--letter-spacing-wide-1);
}
.nav-link:hover .num,
.nav-link.active .num { color: var(--color-accent-subtle); }
```

### Conventions

- **240px wide.** Wider than that and the sidebar starts to dominate; narrower and the labels truncate.
- **Three-part structure**: a mark/header at the top, a list of sections (each with an uppercase section label and 2–4 links), and an active state. No footer chrome.
- **Brand mark at the top** mirrors the status-bar mark — square accent dot + uppercase wordmark. Adds a one-line subtitle (`cluster-a · console`) to ground the user in *which* environment.
- **Section labels** (`OPERATIONS`, `GOVERNANCE`, `SYSTEM`) are in `role-caps` style — wide tracking, tertiary text. They don't link; they're labels.
- **Numbered links** (`[01] Fleet`, `[02] Jobs`) are part of the spec-sheet voice. Numbering resets within sections or runs continuously across all sections — pick one and hold it. Continuous numbering reads more like a console; section-reset reads more like documentation.
- **Active state has three signals**: text color shifts to `--color-accent`, a 2px left border in `--color-accent` appears, and the background lifts to `--color-surface-2`. Don't drop any of them — the redundancy is intentional for scannability.
- **Hover state lifts text to primary and background to surface-2**, but does NOT add the left border. The left border is reserved for "you are here," not "you are hovering."
- **The sidebar is sticky.** It stays in place as the main content area scrolls.

### When NOT to use a sidebar

If the prototype has fewer than 5 destinations, use horizontal tabs instead. A 3-link sidebar reads as overkill — the chrome dominates the content. Sidebars earn their cost when there are enough destinations that horizontal tabs would wrap or scroll.

## 4. Sub-nav

The "filters within a section" pattern. Subordinate to whatever nav got you here.

Two flavors: **filter chips** (pick one or more from a set) and **segmented control** (mutually-exclusive view toggle).

### Filter chips

```html
<div class="subnav-chips">
  <button class="chip active">All <span class="cnt">2,184</span></button>
  <button class="chip">Running <span class="cnt">1,847</span></button>
  <button class="chip">Queued <span class="cnt">312</span></button>
  <button class="chip">Failed <span class="cnt">25</span></button>
</div>
```

```css
.subnav-chips {
  display: flex; gap: 0;
  background: var(--color-surface-1);
  border: 1px solid var(--color-border-strong);
  padding: 4px;
  width: fit-content;
}
.chip {
  background: transparent;
  border: 1px solid transparent;
  padding: 6px 14px;
  font-family: var(--font-family-mono);
  font-size: var(--font-size-11);
  color: var(--color-text-secondary);
  letter-spacing: var(--letter-spacing-wide-1);
  text-transform: uppercase;
  cursor: pointer;
  display: inline-flex; align-items: center; gap: 8px;
}
.chip:hover { color: var(--color-text-primary); }
.chip.active {
  color: var(--color-text-primary);
  background: var(--color-surface-3);
  border-color: var(--color-border-strong);
}
.chip .cnt {
  color: var(--color-text-tertiary);
  font-size: var(--font-size-10);
  letter-spacing: var(--letter-spacing-normal);
  text-transform: none;
  font-variant-numeric: tabular-nums;
}
.chip.active .cnt { color: var(--color-accent-subtle); }
```

### Segmented control

```html
<div class="segmented">
  <button class="seg active">Table</button>
  <button class="seg">Grid</button>
  <button class="seg">Timeline</button>
</div>
```

```css
.segmented {
  display: inline-grid;
  grid-auto-flow: column;
  grid-auto-columns: 1fr;
  background: var(--color-bg);
  border: 1px solid var(--color-border-strong);
}
.seg {
  background: transparent;
  border: 0;
  border-right: 1px solid var(--color-border-strong);
  padding: 7px 16px;
  font-family: var(--font-family-mono);
  font-size: var(--font-size-10);
  color: var(--color-text-secondary);
  letter-spacing: var(--letter-spacing-wide-1);
  text-transform: uppercase;
  cursor: pointer;
}
.seg:last-child { border-right: 0; }
.seg:hover { color: var(--color-text-primary); }
.seg.active {
  background: var(--color-surface-2);
  color: var(--color-accent);
}
```

### Conventions

- **Filter chips** are for filtering a list. Active state lifts the chip surface to `--color-surface-3` with a hairline border. Counts inside chips use `--color-accent-subtle` when active, tertiary text otherwise.
- **Segmented control** is for view-mode switching (table vs. grid vs. timeline). Active state shifts the background to `--color-surface-2` and the text to `--color-accent`.
- **Neither uses aqua.** Aqua remains reserved for primary CTAs. Sub-nav is structural, not actionable in the CTA sense.
- **Sub-nav lives below the section header it belongs to**, with 16–24px of space above it. Don't tuck it inside the `sec-head` element — that's reserved for the numbered label and spec annotation.

## Composition: which nav goes with which

A few canonical compositions to reach for, in order of frequency:

### A — Tabs + sub-nav (most common)

Tabs across the top of a panel, sub-nav within the active tab. Status bar above with breadcrumb. No sidebar. This is the default for an operations console.

```
[ STATUS BAR with breadcrumb                              ]
[ TABS: Fleet · Jobs · Policies · Reports                 ]
  [ SUB-NAV: All · Running · Queued · Failed              ]
  [ MAIN CONTENT                                           ]
```

### B — Sidebar + sub-nav (full app shell)

Sidebar on the left, no top tabs (the sidebar replaces them), sub-nav within the active section. Status bar runs across the top of the main content area.

```
[ NAV ] [ STATUS BAR with breadcrumb                     ]
[ ⋮   ] [ SUB-NAV: All · Running · Queued · Failed       ]
[ ⋮   ] [ MAIN CONTENT                                    ]
```

### C — Tabs only (single-section prototype)

A focused prototype showing one section's worth of navigation. No status bar, no sidebar, no sub-nav. Common in style tiles, demos, and one-screen specs.

```
[ TABS: Overview · Details · History                     ]
[ MAIN CONTENT                                            ]
```

### What to avoid

- **Tabs + sidebar at the same level.** This creates two competing top-level navs. If you have a sidebar, the tabs become a sub-nav-like control *within* a section, not above it.
- **Breadcrumb without a status bar.** The breadcrumb is part of the status bar's grammar. Floating it on its own reads as web-app, not console.
- **More than two levels of nav inside the main content area.** Tabs + sub-nav is the cap. If you need a third level, you need a different page structure.

## Common mistakes

- **Treating tabs like buttons.** This is the most common failure mode. Filling the active tab with teal, using uppercase labels, applying `role-button` styling — all of it makes the tab read as a button (a thing you click to do something) instead of a tab (the indicator that says "this is the page you're on"). Tabs use lowercase labels at body size, regular weight when inactive, and an underline-only active state. The whole row should feel quiet.
- **Aqua active states.** Tabs and sidebar links use teal (`--color-accent`) when active. Aqua is for primary CTAs only.
- **Rounded tabs or pills.** Hard edges only. The system has no `border-radius` tokens.
- **Drop shadows on the sidebar.** Use the right border (`1px solid --color-border-strong`) for separation, not a shadow.
- **Mixing the two sub-nav flavors.** Filter chips and segmented control look superficially similar but have different jobs. If users pick one option from a set of mutually-exclusive choices → segmented. If users filter a list (and might combine filters) → chips.
- **Putting a description under the nav header.** Same trap as the masthead — the nav header is a spec-sheet header, not a marketing page hero. Brand mark + one-line uppercase subtitle is the ceiling.
- **Inventing numbered tabs when the user didn't provide them.** Numbers belong on tabs only when the order is meaningful (steps in a wizard, a defined workflow). Don't sprinkle them on arbitrary sibling pages — they read as forced.

## Density behavior

At `data-density="compact"`:
- Tab padding compresses to `8px 14px`
- Sidebar link padding compresses to `5px 24px`
- Sub-nav chips compress to `4px 10px`

At `data-density="spacious"`:
- Tab padding grows to `12px 22px`
- Sidebar link padding grows to `9px 24px`
- Sidebar width can grow to 280px

Per the foundation skill's first-prompt defaults: standard density unless the user explicitly asks otherwise.

## Reference

The vertical sidebar pattern is rendered live in `index.html` (the design system index page). The horizontal tabs and inline sub-nav patterns are rendered in `examples/sovereign-terminal-style-tile.html` (section 04). The composition variants and a worked sub-nav example are in `examples/navigation-patterns.html`.
