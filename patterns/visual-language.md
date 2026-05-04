# Sovereign Terminal — Visual Language

The unwritten rules. Read this once before building your first Amalgamy prototype, then reference as needed. Tokens tell Claude *what colors and sizes to use*; this doc tells Claude *why and when*.

## What this system is for

Amalgamy LaunchHPC is sovereign HPC infrastructure: enterprises, government, defense, and research customers running large GPU/CPU jobs on dedicated clusters. The product lives at the intersection of three audiences: (a) **operators** who watch fleet health and intervene when jobs fail, (b) **policy authors** who write the scheduling and access rules, and (c) **executive stakeholders** who consume narrative summaries of capacity, utilization, and SLA performance.

Sovereign Terminal is a single visual language that serves all three by leaning hard into one stance: **this is a system console, not a product UI.** The information is the design. Density is high. Chrome is low. Branding is monochromatic and precise.

## The five stances

When in doubt, fall back to these.

### 1. Hard edges, never soft

No border-radius. No box shadows. No gradient fills. Surfaces stack via discrete elevation steps and 1px borders. The visual language is geometric, terminal-derived, and intentionally austere. A rounded corner anywhere on the screen is a defect.

The only deliberate non-rectangular forms are the brand dot in the status bar, individual circular status dots when used decoratively (rare), and the cursor caret in the masthead — and even those are squares, not circles.

### 2. Monospace as voice, not just font

JetBrains Mono is doing more work than typeface choice. It signals: *every character occupies the same horizontal slot. Numbers will line up. Code samples won't reflow. Tables will read as fixed-width data.* Choosing mono for body copy is a choice to make the UI feel like a terminal session — running, structured, precise.

The single allowed exception is **executive narrative** — long-form prose intended to be *read* rather than *scanned*. For that, switch to Inter via `role-narrative` or `role-narrative-lg`. Inter is allowed only for paragraphs of 3+ sentences of prose. Never use Inter for labels, table cells, metric values, or any structured/scannable content.

### 3. Stepped surfaces, never floating

Imagine the screen as a stack of glass plates at four depths:
- `--color-bg` (deepest, the page itself, also "recessed" containers)
- `--color-surface-1` (the default working surface)
- `--color-surface-2` (one step up — selection, modal, hover-lift)
- `--color-surface-3` (top-of-stack — popover, dropdown, row hover)

Movement up or down the stack is a 1-step change in surface color, plus a 1px border. Never a shadow. The depth gradient is shallow — you should be able to look at any screen and see the stepping working hard, not the shadows.

Inversions are allowed and useful: a table header can sit on `--color-bg` while the table body sits on `--color-surface-1`. That reads as "the header is recessed into the panel" — the shallowness is the point.

### 4. Two accents, one hierarchy

Aqua (`--color-accent-secondary`) and Teal (`--color-accent`) are the most-misused tokens in the system. They are not interchangeable.

**Aqua** is the brightest color the system possesses. It is reserved for the **single most important action on the screen** — the primary CTA. If you find yourself using aqua for two buttons, one of them is wrong. If a screen has no primary action, aqua should not appear on it (except as the active state of a primary navigation tab — see below).

**Teal** carries everything brand-present that isn't a primary CTA. Secondary buttons, links, the active dot in the status bar, the masthead cursor, the spine on policy cards, inline `<code>` tokens, sparkline strokes, the active state of dense table tabs.

**Teal-deep** (`--color-accent-subtle`) is the workhorse. It marks structurally important text — section labels, table column headers, ID chips, policy keys, log keys — without being eye-catching. If you need to say "this label is structural, not decorative," teal-deep.

Never use aqua and teal together as a duotone or gradient. They are a hierarchy expressed as two distinct, single-color states.

### 5. Density and atmosphere are global, not local

The two top-level dials — density (compact/standard/spacious) and atmosphere (0–1) — are user-facing controls in the canonical style tile. They are not theming gimmicks; they reflect real product modes:

- **Compact density** is for operations consoles where the user is looking at hundreds of rows and needs to see them all without scrolling.
- **Standard density** is the default, calibrated for sustained reading on a 1440px display.
- **Spacious density** is for executive screens and demos where the audience is consuming, not operating.

- **Atmosphere 0** is clinical — flat surfaces, no glow, no scanlines. Reads as "professional system."
- **Atmosphere ~0.3–0.4** is the production sweet spot — subtle grid, faint scanlines, gentle accent glow. Reads as "premium terminal."
- **Atmosphere 1.0** is full CRT phosphor — heavy bloom, visible scanlines, glowing accents. Reads as "demo theatre" and is appropriate for marketing screenshots and conference talks, less so for daily-use product.

When building a prototype, *pick one density and one atmosphere for the whole prototype*. Don't switch them between screens.

## Component patterns observed in the canonical style tile

These are the recurring constructions worth knowing as starting points.

### Status bar (top-of-screen frame)

A persistent thin bar across the top of full-screen views. Five segments, separated by `gap: 24px`:
1. Brand mark (square accent dot + `AMALGAMY · LAUNCHHPC` in `--color-text-primary`, semibold, wide tracking)
2. Breadcrumb path (mono, `/` separators in `--color-text-tertiary`, active segment in `--color-text-primary`)
3. Connection pill (`● connected` in `--color-success` with hairline border)
4. Endpoint pill (host, in `--color-text-secondary` with hairline border)
5. Timestamp (right-aligned, `--color-text-tertiary`, tabular-nums)

Border: 1px solid `--color-border-strong`. Background: `--color-surface-1`. Padding: 12px 18px.

### Section header (numbered + named)

Used between major sections of any prototype.
- `[01]` numeric tag in `--color-accent-subtle`, `role-secondary`
- Section name in `role-heading`
- Optional right-aligned spec annotation in `role-secondary`, `--color-text-tertiary`
- Underline: `1px dashed --color-border-default`

This frames each section as a numbered specimen — the spec-sheet voice in action.

### Metric tile

Four-up grid, no gap, one outer border, internal cell dividers. Each cell:
- Top: `role-label` with optional teal-deep tag in the right ("GPU Util · *cluster-a*")
- Center: large metric in `role-metric-display` with smaller unit in `role-secondary`-ish styling
- Bottom: trend line — colored delta (`--color-success` / `--color-danger` / `--color-warning`) plus muted comparison text
- Decoration: small SVG sparkline in absolute-positioned bottom-right corner, stroked in `--color-accent` at low opacity

See `components/metrics-SKILL.md` for the worked example.

### Status row (sentinel bar)

Five-up grid showing job/node states. Each cell:
- Glyph (▶ ● ◭ ✕ ∅) tinted by status color
- Uppercase status label in `role-label` with that color's text-variant
- Large count in `role-metric-value`, tabular-nums
- 2px sentinel bar at the bottom in the status color (sub-100% width allowed for "of total" semantics)

The glyph vocabulary matters: ▶ for running, ● for healthy, ◭ for warning, ✕ for critical, ∅ for idle.

### Dense table

32px row height. No zebra striping. Rows separated by 1px solid `--color-border-subtle`. Header on `--color-bg` (recessed), body on `--color-surface-1`. Hover state lifts the row to `--color-surface-3`.

Status cells use the format `■ running` — a 6×6px square in the status color followed by lowercase status text in the matching status text variant. ID cells use the format `job-` (in muted) + `8842a1` (in primary) to push the meaningful suffix forward.

### Policy / rule card

Used for displaying scheduling rules, access policies, or any structured clause. Border on three sides + a 2px **left border in `--color-accent`** (the brand spine). Header includes title (`role-title`), an ID chip in teal-deep on bg, and a status chip on the right (filled with low-alpha status color, hairline border, dot prefix).

Body uses a 2-column grid (`auto 1fr`, 18px gap) of uppercase keys (`WHEN / AND / THEN`) and values containing inline `<code>` chips. Bottom carries a description in `role-body-sm` with `--color-text-secondary` and a max-width of ~78ch.

### Live log / console

The signature pattern. Header strip on `--color-bg` (recessed) with traffic-light dots — but the dots are squares, and the live one is `--color-success` with a low-blur glow. Center text reads `scheduler.log · tail -f`. Body uses a 3-column log line: timestamp (80px, tabular-nums, muted), level (64px, uppercase, status-tinted), message (1fr, with inline `<code>` chips). Line spacing: `line-height: 1.8`. Levels carry the same color vocabulary as status: `INFO` blue, `WARN` gold, `OK` green, `CRIT` brick.

This pattern is the strongest single signature of the language. When in doubt about how to display structured event data — reach for the live log.

### Button set

Four button styles, ordered by importance:
1. **Primary** — aqua fill, deep-bg text, 1px aqua box-shadow + 1px inset white-alpha highlight. The metallic look.
2. **Secondary** — teal fill, deep-bg text. Same shape, no inset highlight.
3. **Tertiary** — transparent fill, 1px teal-deep border, primary text. The "outline" button.
4. **Ghost** — transparent fill, no border, danger text. The "destructive secondary" — cancel, dismiss, delete-with-confirmation.

All buttons are 36px tall, 0 18px padding, `role-button` (uppercase, wide tracking, semibold, mono). All buttons accept an inline `<kbd>` element for keyboard shortcut hints, styled as a tiny inset chip.

### Inline code

`<code>` is a primary citizen. Style: `--color-accent` text on `--color-bg` background, 1px solid `--color-border-strong`, 1px 6px padding, `--font-size-11`. Wrap any policy fragment, ID, path, scheduler expression, or technical token. Don't reserve it for "code samples" — use it whenever the text refers to a system value.

## When to depart from the rules

Three legitimate reasons to break the patterns above:

1. **The user explicitly asks for an executive/marketing screen.** Different rules apply: Inter via `role-narrative-lg`, more whitespace, atmosphere may approach 0 (clinical), and primary CTAs may carry brief subtitles.
2. **A new mood demands it.** Phosphor (amber on black) and Vault (gold on warm black) shift the palette far enough that some contrast and pairing decisions need re-tuning. Test against `--atm: 0.4` to surface palette mistakes.
3. **Density requires reflow.** At compact density, some patterns (like the 5-up status row) may need to shed a column or compress to a single horizontal strip. The numeric/structural information must remain readable.

For any other deviation: reach for the patterns above first, then discuss with the team before forking.

## Reference

Canonical implementation: `examples/sovereign-terminal-style-tile.html`. Open in browser to see the language in motion with all four moods, three densities, and the atmosphere slider live.
