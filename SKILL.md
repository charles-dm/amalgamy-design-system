---
name: amalgamy-sovereign-terminal
description: Foundation token + visual-language skill for Amalgamy LaunchHPC prototypes. Style direction is Sovereign Terminal — a dark, monospace-led, terminal-derived UI for sovereign HPC. Use this skill BEFORE any component skill whenever building any UI for Amalgamy. It defines the token vocabulary (color, typography roles, status palette, surface elevation), the visual rules that tokens alone cannot express (when to use which surface level, primary vs. secondary accent, when Inter is allowed), and the rendering invariants (tabular numerals on metrics, uppercase on labels/caps/buttons, hard-edged borders never shadows). Also use whenever the user wants to add a new mood (Terminal/Phosphor/Arctic/Vault), tune density (compact/standard/spacious), or adjust atmosphere (clinical → CRT bloom).
---

# Amalgamy — Sovereign Terminal

Foundation skill for every Amalgamy LaunchHPC prototype. Defines tokens, visual rules, and rendering invariants. Component skills depend on this; load this **first**, always.

## Core Principle

**Sovereign Terminal is a terminal-derived information system, not a dashboard.** Hard edges, no rounded corners, no drop shadows, monospace everywhere except executive narrative. Borders and surface stepping carry the elevation work that shadows do in lighter design systems. Information density is high; chrome is low.

Components never use hardcoded values. Every color, size, weight, and spacing reference resolves to a CSS variable defined in `tokens/amalgamy-reset.css`. This is what makes mood-swapping (Terminal → Phosphor → Arctic → Vault) and density-swapping (compact/standard/spacious) work — overrides cascade because everything downstream is variables.

## Token File

`tokens/amalgamy-reset.css` is the single source of truth and matches the Figma variable collections name-for-name. Always include it (or its contents) at the top of any artifact. Tokens come in two layers:

- **Primitives** — `--color-bg`, `--font-size-13`, `--font-weight-semibold`, `--letter-spacing-caps` etc. Numeric or descriptive, not semantic.
- **Role aliases + utility classes** — `.role-metric-display`, `.role-label`, `.role-caps` etc. Each composes 5 primitives (family, size, weight, line-height, letter-spacing) and matches one Figma text style 1:1.

**Prefer role utility classes over re-composing primitives.** They mirror Figma, they're shorter, and they bake in the right invariants (tabular nums on metrics, uppercase on labels/caps/buttons).

## Visual Rules (the part tokens cannot express)

These are decisions that hold across every Amalgamy prototype. Treat them as invariants.

### Surface elevation: stepping, not shadowing

Surfaces stack as numeric steps, not as semantic roles. Higher number = closer to the eye.

| Token | Use |
|-------|-----|
| `--color-bg` | Page background. Also used for "recessed" containers (table headers, code chips, log frames) where the surface should read as *behind* the surrounding panel. |
| `--color-surface-1` | The default panel surface — most cards, tables, status rows, log bodies. |
| `--color-surface-2` | Nested elevation — hover-lifted variants, selected items, modal surfaces over `surface-1`. |
| `--color-surface-3` | Top-of-stack — popovers, dropdowns, the active region of an interaction. Also the row hover state on tables. |

**Never use box-shadow for elevation.** Stepping plus a 1px border (`--color-border-default` or `--color-border-strong`) does the work. The only acceptable use of `box-shadow` is the inset-highlight on primary buttons (a 1px inner light line that simulates a metallic edge).

### Border philosophy: present, but quiet

Every panel, table, log, status row, and metric grid has a `1px solid var(--color-border-strong)` outer border. Cells inside grids divide with `1px solid var(--color-border-strong)`. Internal subdivisions (between table rows, between policy header and body) often switch to `1px dashed var(--color-border-default)` — dashed dividers signal *"these are sections of the same thing"*, solid borders signal *"these are separate things"*.

### Two accents, two jobs

This system has **two** brand accents and they are **not interchangeable.**

- **`--color-accent-secondary` (aqua, `#A9F4EA`)** — primary CTAs only. The single most important button on a screen. Light, almost overexposed against the dark background. Pair with `--color-text-inverse` (the deep teal-black) for text inside the button.
- **`--color-accent` (teal, `#2ECEC0`)** — secondary CTAs, links, the active dot in the status bar, the cursor glyph in the masthead, the section-spine on policy cards (a 2px left border), inline `<code>` tokens. Brand-present everywhere.
- **`--color-accent-subtle` (teal-deep, `#1F908A`)** — section labels, table column headers, ID chips, key-value labels. *Brand-tinted but quiet.* Anywhere you want "this is structurally important" without shouting.

Mistake to avoid: using teal for a primary CTA. The aqua/teal pair is a hierarchy, not a palette. If a screen has a primary action, that action is aqua. Everything else uses teal.

### Status vocabulary

Status colors carry product-specific meaning in HPC contexts. Use them by intent, not by hue.

| Token | Means |
|-------|-------|
| `--color-info` (sky blue) | **running** — actively executing |
| `--color-success` (sea green) | **healthy / done / ok** — completed or stable |
| `--color-warning` (warm gold) | **warn / throttled / degraded** — running but compromised |
| `--color-danger` (brick) | **critical / failed** — broken or stopped abnormally |
| `--color-neutral` (cold neutral) | **idle** — present but not doing anything |
| `--color-status-queued` (dusty periwinkle) | **queued** — waiting for resources |

For text on these colors at small sizes, prefer the `-text` variants (`--color-success-text`, `--color-warning-text`, etc.) — they're tuned for legibility on the dark surface and avoid the slight neon cast of the pure status hues.

A status indicator is almost always a **6×6px or 8×8px square** of the status color, never a circle. Squares match the hard-edged language; circles read as foreign.

### Typography: monospace by default, Inter by exception

JetBrains Mono is the system font. It runs in display sizes (48px metric displays), title sizes, body, captions, and labels. **The only legitimate use of Inter is `role-narrative` and `role-narrative-lg`**, reserved for executive narrative screens — long-form prose that would be punishing in mono. Never mix Inter and Mono on the same line.

### Tabular numerals: always, on metrics

Any numeric token in a metric, status count, table cell, log timestamp, or anywhere numbers might align vertically gets `font-variant-numeric: tabular-nums`. The `role-metric-display` and `role-metric-value` utilities bake this in. If you compose a custom numeric style, add it explicitly.

### Uppercase: labels, caps, buttons

`role-label`, `role-caps`, and `role-button` ship with `text-transform: uppercase` baked in. The wide tracking on `--letter-spacing-caps` (0.16em) and `--letter-spacing-wide-2` (0.10em) is calibrated for uppercase rendering — do not apply it to mixed case.

### Icons: Lucide, via CDN, sparsely

Iconography is **[Lucide](https://lucide.dev)** loaded from a CDN — never as a font, never as a custom set. Add this once per page:

```html
<script src="https://unpkg.com/lucide@latest"></script>
<script>lucide.createIcons();</script>
```

Then inline icons with `<i data-lucide="terminal"></i>`. Defaults come from the tokens file: 14px, stroke-width 2, vertical-aligned with body text. Three sizes: default (14px), `.icon-sm` (12px, with caps/labels), `.icon-lg` (16px, with body/headings). Recolor with `.icon-accent`, `.icon-aqua`, `.icon-subtle`, `.icon-faint` — or set `color` on the parent (Lucide icons inherit `currentColor`).

**Use icons only where they carry information text alone cannot.** That means: brand sigils, scannable status indicators, directional affordances on buttons, file/format markers. It does **not** mean: decorating headings, prefixing every nav item, illustrating the obvious. The system is text-led — most prototypes ship with fewer than ten icons total. If a Sovereign Terminal screen looks icon-heavy, it has stopped being Sovereign Terminal.

Stroke weight is a hard floor: never go below 1.5. Lucide ships at 2, which is correct for our scale; thinner strokes go invisible against the dark surface.

### Density and atmosphere are global controls

Two top-level dials drive the whole system:

- **Density** (`data-density="compact|standard|spacious"`) rescales padding, gaps, and type via a multiplier. Always honor it — never hardcode pixel padding outside the multiplier system.
- **Atmosphere** (`--atm: 0..1` on body) controls the CRT-bloom layer: grid overlay opacity, scanline visibility, and accent glow. `0` is clinical, `1` is full phosphor terminal. Most production prototypes sit between `0.2` and `0.4`.

Components should not fight these. If a component is rendered with `--atm: 0.4` it should automatically pick up scanlines and glow without the component having to know.

## Token Reference (quick)

### Surface
- `--color-bg` `#011619` — page, recessed
- `--color-surface-1` `#0A2125` — default panel
- `--color-surface-2` `#143034` — elevated/nested
- `--color-surface-3` `#1A3A40` — top-of-stack, hover

### Border
- `--color-border-subtle` `#1E3A40` — internal/dashed dividers
- `--color-border-default` `#284C53` — standard
- `--color-border-strong` `#284C53` — outer panel borders

### Text
- `--color-text-primary` `#DCE8E6` — body, headings
- `--color-text-secondary` `#8AA6A4` — meta, captions on emphasized rows
- `--color-text-tertiary` `#54706E` — timestamps, faint meta
- `--color-text-faint` `#2A4448` — disabled, decorative
- `--color-text-inverse` `#011619` — text on aqua/teal buttons

### Brand
- `--color-accent-secondary` `#A9F4EA` — **primary CTAs only**
- `--color-accent` `#2ECEC0` — secondary CTAs, links, brand presence
- `--color-accent-hover` `#45D9CD` — accent hover
- `--color-accent-subtle` `#1F908A` — labels, IDs, code

### Status
- `--color-info` `#69B1E8` (running) · `--color-success` `#0DC378` (healthy/done) · `--color-warning` `#C99A3F` (warn/throttled) · `--color-danger` `#C8533B` (critical/failed) · `--color-neutral` `#3E5A5E` (idle) · `--color-status-queued` `#8EA0C9` (queued)
- `-text` variants (`--color-success-text`, `--color-warning-text`, `--color-danger-text`, `--color-info-text`, `--color-neutral-text`) are tuned for small-size legibility on dark.

### Data viz
- `--color-viz-1` `#D11A6E` (magenta) · `--color-viz-2` `#E5C739` (amber) · `--color-viz-3` `#9988C2` (violet)

### Typography roles (use the utility classes)
`role-display` · `role-title` · `role-heading` · `role-body` · `role-body-sm` · `role-secondary` · `role-metric-display` · `role-metric-value` · `role-label` · `role-caps` · `role-button` · `role-code` · `role-kbd` · `role-narrative` · `role-narrative-lg`

## Rendering Invariants (do these by default)

Every component built on this system, unless overridden for a deliberate reason:

1. **Imports `tokens/amalgamy-reset.css`** at the top of the artifact (or inlines its full contents).
2. **Uses role utility classes** for type. `<div class="role-metric-display">97.4%</div>` is preferred over re-stating `font-family / size / weight / line-height / letter-spacing`.
3. **Uses CSS variables for color and border**, never hex codes.
4. **Has hard-edged corners** (`border-radius: 0`). The system has no `--radius-*` tokens and that is intentional. The only exceptions are: the JetBrains Mono cursor block in the masthead (no border-radius applied — it's a square block), and individual circular dots which are deliberately decorative.
5. **No drop shadows** — see Surface elevation above.
6. **Spec annotation in the corner** — many panels in the style tile carry a small uppercase spec line (e.g., `cell · spark · semantic delta`) styled with `role-secondary` + `--color-text-tertiary`. Carry this convention forward where it fits — it's part of the "this is a spec sheet, not a marketing page" voice.

## Creating a New Mood

To add a new client variant or mode:

1. Add a key to the `MOODS` object in the prototype JS, defining override values for the same set of variables that `terminal` defines.
2. The variable surface is fixed: surfaces (4), borders (3), text (4), brand (3), status (6), viz (3). Provide all of them.
3. Provide a 5-color preview swatch (`preview: [bg, surface, accent, accent-secondary, success]`) for the mood picker.
4. Test the mood at `--atm: 0.4` — atmosphere amplifies hue choices and tends to surface palette mistakes that look fine at `0`.

The four canonical moods are: **Terminal** (default — teal/aqua on near-black), **Phosphor** (amber on black, vintage CRT), **Arctic** (icy blues, daylight-ish), **Vault** (gold on warm black, archival).

## Density: not a free parameter

Density is for fitting more or less information on screen — it is not a brand lever. Most production prototypes ship in **standard**. Use **compact** for power-user screens (operations dashboards, tables of 100+ rows). Use **spacious** for executive narrative screens, onboarding, or presentation-mode demos. Don't mix densities within a single screen.

## Voice and Texture

The system reads as: spec sheet, system console, scheduler trace, policy clause. It does **not** read as: marketing page, hero illustration, lifestyle photography, product launch. Concretely:

- Section headers use `[01]` `[02]` numeric labels in `--color-accent-subtle`, never uppercase brand wordmarks.
- Inline `<code>` is an honest semantic — wrap policy fragments, IDs, paths, expressions. Style them with `--color-accent` text on `--color-bg` background, 1px border. `code` is part of the prose, not an exception.
- Dashed dividers (`border-style: dashed`) signal "section of the same artifact" and appear inside policy cards, between metric trend lines and values, etc. Solid borders separate distinct artifacts.
- Status bars across the top of screens (`AMALGAMY · LAUNCHHPC | breadcrumb | connected | api.host | timestamp`) are a recurring frame. Reach for them on full-screen views.

## Component Skills

Component skills extending this foundation live in `components/`. Each has its own `SKILL.md` and demos. As of revision 0.1: `metrics-SKILL.md` is provided as a worked example. Future skills will cover status displays, dense tables, log/console patterns, navigation, buttons, policy cards, and data viz.

When asked to build a component that doesn't have a skill yet, build it using the rules above and the canonical examples in `examples/` as reference.
