---
name: amalgamy-design-system
description: >
  Design system skill for Amalgamy / LaunchHPC prototypes and production HTML.
  Governs all UI generation — component selection, layout composition,
  token usage, and interaction patterns. Load at the start of every
  prototype session. Prevents drift by anchoring decisions to
  explicit rationale, not just component specs.

  v0.2 — May 2026
  Typeface committed: Geist + Geist Mono
  Typography reconciled: 28 styles, all leading/tracking standardized
  Brand palette complete: teal surfaces, status system, data viz
  Workflow nodes added: LaunchHPC job lifecycle patterns
---

# Amalgamy Design System — Claude Skill
## Version 0.2 — May 2026

## How to use this skill

Read this file completely before generating any HTML, CSS, or layout.
When a prompt asks you to build a screen, flow, or component:

1. Identify the **user role** (operator, researcher, executive)
2. Identify the **screen type** (dashboard, list+detail, form, settings)
3. Select the correct **layout template** from Section 3
4. Select components using the **rationale rules** in Section 4
5. Reference only the **token names** defined in Section 2 — never hardcode values
6. Apply the **interaction rules** in Section 5

If something is not defined here, default to the most structurally
conservative option and add a comment flagging it for review.
Do not invent visual treatments. Do not hardcode colors, sizes, or fonts.

---

## 1. Product and persona context

### What Amalgamy is

LaunchHPC is an intent-based workload scheduling platform for heterogeneous
compute — GPUs, CPUs, edge devices — with late-binding hardware allocation.
Users describe what they want to accomplish; the system decides how to run it.

**Core technical concepts every UI decision must reflect:**

- **Late binding** — hardware not assigned until the exact moment data is
  local and compute is ready. Slurm locks all GPUs at submission;
  LaunchHPC defers. Any UI that shows "hardware assigned" before this moment
  is misleading.

- **Scatter-gather** — computation broken into parallel shards, sent to
  multiple nodes simultaneously, gathered when all complete. The researcher
  sees one job; the system runs many. Visualization must show parallelism.

- **Data gravity** — large datasets stay where they are; compute moves to
  data. Data locality drives hardware selection. Never imply data moves
  freely or cheaply.

- **Slurm plugin model** — LaunchHPC initially runs invisibly under Slurm
  at Texas Tech. Researchers keep using srun/sbatch. The Slurm nudge
  (CLI text injection) is the primary researcher touchpoint.

- **Decision Ledger** — git-backed audit record of every scheduling decision.
  Serves three audiences with different reference ranges. Every decision
  is auditable. The UI must surface this without overwhelming.

- **Rules engine** — four-level policy hierarchy: global → tenant → team → user.
  Operator defines resource pools. Policy overrides are graceful (give
  highest available and explain why). Always show what policy was applied.

### The three personas

**Cluster Operator**
- Mental state: scanning for problems, managing incidents, Monday-morning triage
- Context: desktop, 1440px, multiple tabs open, high information density expected
- Goal: know fleet health at a glance, drill into problems fast
- Key screens: Fleet overview, node detail, job queue, alert feed, tenant dashboard
- What they fear: missing a critical failure buried in noise
- Design principle: signal over decoration — every element earns its place
- Density: comfortable with table/cell-md at 36px rows, expects no whitespace padding

**HPC Researcher**
- Mental state: focused, task-oriented, terminal-native, skeptical of abstraction
- Context: desktop or laptop, terminal running alongside the UI
- Tools: vi/emacs, Jupyter, srun/sbatch, sacct, tail -f
- Goal: submit a job, understand its status, diagnose failures without guessing
- Key screens: Job submission, job status, job detail, output logs, ledger entry
- What they fear: not knowing why something is slow or failed; system hiding decisions
- Design principle: progressive disclosure — enough to diagnose, not a manual
- Trust is earned incrementally. Never force new UI on them. The Slurm nudge
  is the onboarding mechanic — a single line in CLI output, not a modal.

**Executive / Buyer**
- Mental state: occasional, preparing for a board meeting or review call
- Context: laptop, possibly tablet, not a daily user
- Goal: utilization trending up, ROI provable, ESG metrics visible
- Key screens: Utilization summary, cost efficiency, trend charts, yield report
- What they fear: being asked a question they can't answer in a meeting
- Design principle: narrative over data — tell the story, not every number
- Use body/narrative text, large metrics, generous whitespace

---

## 2. Token reference

All generated CSS must use these token names. Never hardcode hex, px, or
font names in component or layout styles. Values live in `amalgamy-reset.css`.

### Color tokens

```
Background hierarchy (darkest to lightest):
  --color-bg              Page canvas (#051315)
  --color-surface-1       Cards, sidebar (#0A181A)
  --color-surface-2       Table rows, hover bg (#0F1E20)
  --color-surface-3       Pressed states, code bg (#162526)
  --color-surface-code    Code blocks, terminal (#0D1A17)

Border scale (teal-tinted white at opacity):
  --color-border-subtle   Dividers, table rows (#162526)
  --color-border-default  Card edges, inputs (#223233)
  --color-border-strong   Focus rings, active (#456066)
  --color-border-kbd      Keyboard shortcut container (18%)

Text hierarchy (WCAG AA on surface/1):
  --color-text-primary    Headings, labels, primary content
  --color-text-secondary  Supporting text, descriptions
  --color-text-tertiary   Timestamps, metadata, labels
  --color-text-faint      Decorative only — do not use for readable content
  --color-text-inverse    Text on teal or light backgrounds

Brand (Amalgamy teal):
  --color-accent          Primary actions, links, active nav (#2ECEC0)
  --color-accent-subtle   Teal background fill (12% opacity)
  --color-accent-hover    Hover state (#45D9CB)
  --color-accent-active   Pressed state (#27B5A3)
  --color-accent-muted-bg Deep teal bg for selected states (#081f20)
  --color-aqua            Secondary accent (#A9F4EA)
  --color-aqua-hover      (#B3F5ED)
  --color-aqua-active     (#90CFC7)
  --color-aqua-disabled   (#65938F)

Semantic status (each: default · subtle · text · bg):
  --color-success / --color-success-subtle / --color-success-text / --color-success-bg
  --color-warning / --color-warning-subtle / --color-warning-text / --color-warning-bg
  --color-danger  / --color-danger-subtle  / --color-danger-text  / --color-danger-bg
  --color-info    / --color-info-subtle    / --color-info-text    / --color-info-bg
  --color-queued          Queued job state (#8EA0C9)
  --color-queued-bg       (#1A2830)
  --color-idle-fill       Idle node fill (#2E4445)
  --color-idle-text       (#649497)
  --color-idle-bg         (#0B1A1C)
  --color-danger-hover    (#FE5D52)
  --color-danger-active   (#E63B2F)
  --color-neutral / --color-neutral-text

Data viz palette (use in order, always label):
  --color-dv-1  teal    primary series
  --color-dv-2  amber   secondary
  --color-dv-3  magenta tertiary
  --color-dv-4  violet
  --color-dv-5  sky
  --color-dv-6  coral
  --color-dv-7  lime
  --color-dv-8  lavender
  --color-dv-seq-1 → seq-7   Sequential teal ramp (dark → light)
  --color-dv-div-*            Diverging cool/neutral/warm (5 tokens)
```

### Spacing tokens (8pt base grid)

```
--space-1    4px    icon gaps, tight inline spacing
--space-2    8px    button px, badge pad, kbd pad
--space-3   12px    form field gap, card inner gap
--space-4   16px    default content padding
--space-5   24px    card padding, grid gutter
--space-6   32px    panel padding, between major sections
--space-7   48px    large layout gaps
--space-8   64px    page margin, hero padding
```

Rule: internal spacing ≤ external spacing. Never mix density levels in a table.
Never use three density levels on the same screen.

### Typography tokens

```
Font families (committed May 2026):
  --font-sans    'Geist', 'Inter', system-ui
  --font-mono    'Geist Mono', 'JetBrains Mono', 'SF Mono', monospace

Font sizes (real px — matched to Figma text styles):
  Micro:
    --text-9    9px    table/caps-sm
    --text-10  10px    table/caps-md, table/cell-sm
    --text-11  11px    ui/kbd, table/caps-lg, table/cell-md, ui/code-sm
  Small:
    --text-12  12px    body/xs, ui/label, ui/caps, heading/xs
    --text-13  13px    table/cell-lg
  Body:
    --text-14  14px    body/sm, ui/code, ui/button, heading/group
    --text-16  16px    body/md, heading/card-sm
    --text-18  18px    body/narrative, heading/card
  Heading:
    --text-20  20px    heading/section, body/narrative-lg
    --text-24  24px    heading/subtitle
    --text-32  32px    heading/title
  Display:
    --text-40  40px    display/sm OR metric/value (see disambiguation below)
    --text-48  48px    display/md
    --text-56  56px    metric/display
    --text-64  64px    display/lg
  Legacy aliases: --text-xs → --text-11, --text-sm → --text-12,
    --text-base → --text-14, --text-md → --text-16, --text-lg → --text-18

Font weights (CORRECTED from v0.1):
  --weight-regular:   400
  --weight-medium:    500
  --weight-semibold:  600   ← was incorrectly 700 in v0.1
  --weight-bold:      700   ← NEW — table caps, button labels

Line heights (unitless — no px):
  --leading-tight    1.10   display, metric
  --leading-140      1.40   table/cell-sm
  --leading-145      1.45   table/cell-md
  --leading-normal   1.50   body, headings, UI
  --leading-138      1.38   table/cell-lg
  --leading-relaxed  1.70   narrative, code blocks

Letter spacing (named by role):
  --tracking-tightest  -0.025em   display
  --tracking-tight     -0.01em    heading/title, subtitle
  --tracking-normal     0
  --tracking-wide      +0.04em    ui/button
  --tracking-caps-sm   +0.08em    ui/label
  --tracking-caps-md   +0.10em    table/caps-md
  --tracking-caps-lg   +0.06em    table/caps-lg
  --tracking-caps-xl   +0.16em    ui/caps

TYPE DISAMBIGUATION — critical rules:
  40px editorial text  → .type-display-sm  (Geist Regular)
  40px data value      → .type-metric-value (Geist Mono Medium)
  18px heading         → .type-heading-card (Geist Medium, 150% lh)
  18px body text       → .type-body-narrative (Geist Regular, 170% lh)
  16px heading         → .type-heading-card-sm (Geist Medium)
  16px body text       → .type-body-md (Geist Regular)
  14px heading         → .type-heading-group (Geist Medium)
  14px body text       → .type-body-sm (Geist Regular)
  12px label/input     → .type-heading-xs (Geist Medium) — NEW
  12px body text       → .type-body-xs (Geist Regular)
  De-emphasized body   → .type-body-sm + color: var(--color-text-secondary)
                         NOT a separate style — body/secondary was removed

Text styles removed in v0.2 (do not use):
  body/secondary — merged into body/sm (was identical)
  body/lg        — merged into body/narrative (was identical)
```

### Radius tokens

```
--radius-sm     3px     kbd, badge, tooltip
--radius-md     6px     input, button, chip
--radius-lg     8px     card, modal, panel
--radius-xl    12px     drawer, large card
--radius-full  9999px   pill, avatar, status dot
```

Rule: nested elements always use a smaller radius than their parent.

### Animation tokens

```
--duration-fast    100ms   button press, toggle, row hover
--duration-base    150ms   tooltips, small state changes
--duration-slow    250ms   panels, sidebars, expanding sections
--duration-slower  400ms   page transitions

--ease-out     entering elements
--ease-in      leaving elements
--ease-in-out  repositioning
```

Animate only `transform` and `opacity`. Never `width`, `height`, `top`, `left`.

---

## 3. Layout templates

These are the valid page compositions. Use exactly one per page.
Do not invent new shell structures.

### Template A — Operator dashboard

**Use when:** Persona is Cluster Operator, screen is fleet overview or metrics.

```html
<div class="app-shell">
  <header class="topbar"> ... </header>
  <nav class="sidebar"> ... </nav>
  <main class="main">
    <div class="page-header"> ... </div>
    <div class="page-body page-body--full">
      <div class="dashboard-grid">
        <!-- 4 metric cards across: GPU util, kW/token, active jobs, alerts -->
      </div>
      <div class="chart-grid">
        <!-- 2/3 chart left (utilization over time), 1/3 summary right -->
      </div>
      <div class="table-container">
        <!-- Job queue or fleet table below charts -->
      </div>
    </div>
  </main>
</div>
```

**Density rule:** Dense but not chaotic. Use table/cell-md (36px rows) as default.
Upgrade to table/cell-lg (44px rows) only for the primary fleet table.
The operator expects to see a lot — don't pad for aesthetics.

### Template B — Researcher list + detail

**Use when:** Persona is Researcher, screen is job queue, job status, or
pipeline view.

```html
<div class="app-shell">
  <header class="topbar"> ... </header>
  <nav class="sidebar"> ... </nav>
  <main class="main">
    <div class="page-header"> ... </div>
    <div class="page-body page-body--full">
      <div class="split-layout">
        <div class="split-layout__list"> ... </div>
        <div class="split-layout__detail">
          <div class="detail-panel"> ... </div>
        </div>
      </div>
    </div>
  </main>
</div>
```

**Density rule:** List side is dense (researcher scanning job names and states).
Detail side is airy — they're reading and diagnosing. The pipeline strip
lives in the detail panel, not the list.

### Template C — Executive summary

**Use when:** Persona is Executive, screen is utilization report or ROI summary.

```html
<div class="app-shell">
  <header class="topbar"> ... </header>
  <nav class="sidebar"> ... </nav>
  <main class="main">
    <div class="page-header"> ... </div>
    <div class="page-body">
      <div class="section-stack">
        <!-- Narrative sections, large charts, Decision Ledger executive panel -->
      </div>
    </div>
  </main>
</div>
```

**Density rule:** Generous. Use --space-7 and --space-8 between sections.
Use .type-body-narrative for paragraphs. Every chart tells one story.
No raw data tables. The Decision Ledger executive panel is the primary
data-dense element allowed here.

### Template D — Single workflow / form

**Use when:** Screen is job submission, migration setup, policy creation.
Single focused task.

```html
<div class="app-shell">
  <header class="topbar"> ... </header>
  <nav class="sidebar"> ... </nav>
  <main class="main">
    <div class="page-header"> ... </div>
    <div class="page-body page-body--narrow">
      <!-- Single column, max-width 720px -->
      <!-- Step indicator at top if multi-step -->
    </div>
  </main>
</div>
```


---

## 4. Component rationale rules

### Buttons

**Use `.btn` (default):** Supporting actions — Cancel, Back, Export, View Details.

**Use `.btn--primary`:** Single most important action per section — Submit Job,
Confirm Migration, Apply Changes. One per screen section.

**Use ghost/text-only:** Low-priority actions — Reset filters, Clear selection, Dismiss.

**Sizing rule:**
- Default (32px): toolbar actions, table row actions, secondary actions
- Large `.btn--lg` (40px): primary CTA in page headers, form submit
- Small `.btn--sm` (28px): dense table cells, badge-adjacent actions

**Button label rule:** Mono Bold Uppercase is the Amalgamy button convention.
This is intentional — terminal/CLI signal. All labels use .type-ui-button.
Keep to 3–4 words maximum.

**Disabled state rule:** Only disable when you can explain why. If you can't
surface the reason, keep enabled and show an error on attempt.

### Status indicators

**`.status-dot`:** Live state that changes — Running, Queued, Failed, Idle.
Always pair with text label. Never color alone (accessibility).

**Workflow node states:**
```
```

```

### Data display

**`.data-value` (monospace + tabular-nums):**
Any numeric measurement, ID, timestamp, or technical parameter.
GPU utilization %, kW/token, job ID, node address, duration, memory GB.

**`.data-value--hero`:** One primary metric per card. One per section.

**Table density selection:**
```
table/cell-lg (44px)  operator fleet overview — primary table
table/cell-md (36px)  most operator tables — default
table/cell-sm (28px)  audit logs, Decision Ledger entries, event feeds
```
Never mix density levels in one table. Choose based on row height, not data volume.

**Column alignment rule:**
Text: left-align. Numbers: right-align. Status: center. Headers match data.

### Workflow-specific components

**Slurm nudge (CLI injection):**
One line injected into srun/sbatch output. Formatted as:
`━━ LaunchHPC ━━━━━━━━━━━━━━━━━━━━`
`  [plain language summary of what Amalgamy did and why]`
`  Details: launch.hpc/{job_id}`

Do not build modal alerts for Slurm nudge. It lives in the terminal,
not the UI. The URL is the bridge to the full ledger view.

---

## 5. Interaction rules

### Navigation

Sidebar shows current location with `.nav-item--active`. Exactly one active
item at all times. Clicking nav item: instant, no animation.
Sidebar collapse: `.app-shell--collapsed`, 240px → 64px.

### Loading states

Never use a spinner for content with structural shape. Use skeleton screens.
Skeleton rule: same class structure as loaded state, pulsing placeholders.
Only use spinner for unknown-duration actions (job submission, export).

### Error states

Every error must answer: what happened, why, what to do.
Use `--color-danger` + `--color-danger-bg` for borders and fills.
Never just red text — full inline alert with icon and recovery action.

### Workflow transitions

Pipeline step activation: 150ms ease-out on border-color and background.
Scatter-gather shard completion: 250ms ease-out on the track bar fill.
Decision Ledger panel switch: instant — the researcher doesn't need animation
when switching between audience views. Speed matters.

---

## 6. What not to do

- **Do not hardcode any color, spacing, or font.** All values reference tokens.
- **Do not use body/secondary or body/lg.** They were merged in v0.2.
  Use body/sm (with text-secondary color) or body/narrative respectively.
- **Do not use font-weight 700 for semibold.** weight/semibold = 600.
  Use weight/bold (700) only for table caps and button labels.
- **Do not use Inter or JetBrains Mono.** Typeface is Geist + Geist Mono.
- **Do not convey status with color alone.** Always pair with a text label.
- **Do not mix table density levels** in a single table.
- **Do not use a table for a single item.** Use .detail-pair key-value layout.
- **Do not generate inline styles for layout.** Use layout.css classes.
  Steps have variable content height; columns fight this.
- **Do not show the Decision Ledger as tabs.** Show all three panels side
  by side in the .ledger grid. The researcher sees their view, but the
  panel shape tells them three audiences exist.
- **Do not animate layout properties.**
- **Do not build empty states without a title, description, and action.**
- **Do not show a placeholder-only input label.**

---

## 7. Session startup checklist

- [ ] `amalgamy-reset.css` v0.2 is loaded
- [ ] `amalgamy-layout.css` v0.2 is loaded
- [ ] Persona identified: operator / researcher / executive
- [ ] Layout template selected: A / B / C / D / E
- [ ] All token references use variable names from Section 2

---

## 8. File structure + component library

### Core files (load every session)
```
amalgamy-reset.css     ← All design tokens + base element styles. Load first.
amalgamy-layout.css    ← App shell, grid, page templates. Load second.
skill/SKILL.md         ← This file. Load at session start.
```

### Component files (load only when needed)

Load the relevant component file before generating that component.
Do not load all component files — only the ones the current session requires.

| File | Load when building... |
|---|---|
| `components/component-button.md` | Any button — CTA, action, toolbar, icon-only |
| `components/component-badge-tag.md` | Status indicators, category labels, filter chips |
| `components/component-table.md` | Any list of comparable items — fleet, job queue, audit log |
| `components/component-input-form.md` | Any form — job submission, policy config, search |
| `components/component-card.md` | Metric cards, detail panels, modals, alert boxes |
| `components/component-nav.md` | Sidebar, topbar, tabs, breadcrumbs, app shell |
| `components/component-feedback.md` | Inline alerts, toasts, tooltips, empty states |

### Component file anatomy (what each file contains)
Each component file has:
1. **When to load** — exact trigger conditions
2. **Figma source** — component set name, variant count, file reference
3. **Variants** — table of all Figma properties and their meaning
4. **HTML anatomy** — copy-ready markup for each variant and state
5. **CSS** — complete styles using only token references (no hardcoded values)
6. **Rules** — explicit prohibitions and required patterns

### Full file tree
```
amalgamy/
  amalgamy-reset.css
  amalgamy-layout.css
  amalgamy-style-layer.css  (to author — visual personality overrides)
  skill/
    SKILL.md
  components/
    component-button.md
    component-badge-tag.md
    component-table.md
    component-input-form.md
    component-card.md
    component-nav.md
    component-feedback.md
```

---

## 9. Change log

### v0.2 — May 2026

**Typography (breaking changes):**
- Typeface: Inter → Geist, JetBrains Mono → Geist Mono
- `body/secondary` removed — use `body/sm` + `color: var(--color-text-secondary)`
- `body/lg` removed — use `body/narrative`
- `heading/xs` added — 12px Geist Medium for form labels
- All line-heights standardized to unitless (no px values)
- `--tracking-caps-sm/md/lg` added, replacing ad-hoc tracking values
- `--weight-semibold` corrected: 600 (was 700)
- `--weight-bold` added: 700

**Colors (new):**
- Full Amalgamy surface stack: `--color-bg` through `--color-surface-code`
- Brand teal palette: `--color-accent` + hover/active/muted-bg/disabled
- `--color-aqua` secondary accent
- Complete status system with bg variants
- Data viz palette: `--color-dv-1` through `--color-dv-8`
- `--color-border-kbd` for keyboard shortcut containers

**Components (new):**
- `kbd` styled with container tokens
- `.code-block` and `.code-block--sm` with `.syn-*` syntax roles
- `.type-*` utility classes — one per Figma text style
- `.data-value--display` for 56px hero metrics
- `- `.divider--group` for heading/group separator pattern

**Layout (new):**
- Primary design target updated: 1440px (was 1280px)

---

## 10. Experiment notes (for Chuck and Charles)

The v0.2 skill extends the v0.1 hypothesis — rationale-first documentation
reduces model drift — with three new layers:

**Workflow nodes:** The LaunchHPC job lifecycle is now codeable. The pipeline
strip, conductor score, and Decision Ledger have defined classes and rules.
Watch for: does Claude correctly choose between strata vs pipeline strip
for different researcher contexts?

**Disambiguation table (Section 2 typography):** Four pairs of styles at
identical sizes are now explicitly differentiated. Watch for: does Claude
apply heading/card (18px Medium) vs body/narrative (18px Regular) correctly
without being told explicitly?

**Three-audience ledger:** The Decision Ledger's three-panel pattern is the
most novel component. Watch for: does Claude maintain the reference-range
framing (same value, different normal range) or default to showing three
different values?

Flag any moment where a prompt couldn't be satisfied by this skill as written.
Those gaps are the next sprint's documentation work.
