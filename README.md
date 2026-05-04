# Amalgamy Design System — Team Guide

The Sovereign Terminal visual language, packaged as Claude Code skills. This is what your teammates load into Claude when they're building Amalgamy LaunchHPC prototypes so the output looks consistent across the team.

## What's in this package

```
amalgamy-design-system/
├── SKILL.md                          ← Foundation skill. Load this first, always.
├── README.md                         ← This file.
├── tokens/
│   └── amalgamy-reset.css            ← All 70+ tokens, role utilities, base reset.
├── patterns/
│   └── visual-language.md            ← The "why" — philosophy, stances, conventions.
├── components/
│   └── metrics-SKILL.md              ← Worked example. Pattern for future skills.
└── examples/
    └── sovereign-terminal-style-tile.html
                                      ← Canonical reference. Open in browser.
```

## How to install for the team

**Recommended (shared via git):** Add this folder to a shared repo your team clones into `.claude/skills/amalgamy/` in each project. Claude Code auto-discovers `SKILL.md` files there. One source of truth, everyone stays in sync.

**Project-level:** Drop the entire folder into the project at `.claude/skills/amalgamy/`. Fine for one-off prototypes, doesn't scale to multi-project teams.

**Claude.ai (web):** Paste the contents of `SKILL.md` plus `tokens/amalgamy-reset.css` at the top of your prompt, or attach them as project knowledge. For component work, also include the relevant `components/*-SKILL.md`.

## How teammates use it

A teammate opens Claude Code in an Amalgamy project and prompts something like:

> Build a fleet status page with a status bar header, four-up metric grid (GPU util, active jobs, queue depth, SLA), a status row showing node states, and a dense table of the 10 most recent jobs.

Claude loads `SKILL.md` (foundation), `components/metrics-SKILL.md` (because metrics were requested), references the canonical example, and produces a prototype that already speaks Sovereign Terminal — no need for the teammate to specify colors, type, or layout patterns.

If a teammate asks for something not yet covered by a component skill, Claude falls back to the foundation rules + the canonical example. That's why the example file is bundled — it's a working specimen Claude can pattern-match against.

## How to extend it

Two situations come up:

**Adding a new component skill.** Use `components/metrics-SKILL.md` as the template. Each component skill should: (a) start with frontmatter (`name`, `description`) so Claude can find it, (b) state when to use it vs. when not to, (c) provide the canonical structure as code + token bindings, (d) call out common mistakes, (e) reference the canonical example. Aim for the same length and depth as the metrics skill.

Component skills worth writing next, in rough priority order:
- `status-displays-SKILL.md` (status row, status chips, status bars)
- `tables-SKILL.md` (dense table, log/console table)
- `logs-SKILL.md` (the live log pattern — one of the strongest signatures)
- `buttons-SKILL.md` (4-tier button hierarchy + kbd hints)
- `policy-cards-SKILL.md` (the structured-clause card with 2px brand spine)
- `navigation-SKILL.md` (status bar, tabs, breadcrumbs)
- `data-viz-SKILL.md` (sparklines, full charts using viz palette)

**Adding a new mood.** Edit the `MOODS` object in your prototype JS (or, better, factor it into a separate `themes/<moodname>.js` file). Provide overrides for every variable in the standard mood signature: 4 surfaces, 3 borders, 4 text colors, 3 brand colors, 6 status colors, 3 viz accents. Test at `--atm: 0.4` — atmosphere amplifies palette mistakes.

## A word on what NOT to add

The temptation is to keep adding tokens. Resist it.

- **No border-radius tokens.** Hard edges are part of the language. Every prototype has `border-radius: 0` everywhere except deliberate decorative elements.
- **No shadow tokens.** Surface stepping handles elevation. The only `box-shadow` in the system is the inset highlight on primary buttons.
- **No new color roles for "primary," "secondary," etc.** The two-accent hierarchy (aqua = primary CTA only, teal = everything else brand-present) is a deliberate constraint. Adding a third tier collapses the hierarchy.
- **No new font families.** Mono for everything except `role-narrative` (Inter, executive prose only).

If a teammate is asking to add one of the above, that's usually a sign they're trying to import a different design system's habits. Refer them to `patterns/visual-language.md`.

## Versioning

Current revision: **0.1** (matches the style tile footer).

When making changes that affect rendering across existing prototypes, bump the version in the `SKILL.md` frontmatter and note the change here. Backward-compatible additions (new component skills, new moods) are safe at any time.

## Questions / contributions

The canonical example is the truth on the ground. If a question comes up that the docs don't answer, check `examples/sovereign-terminal-style-tile.html` first — the answer is usually visible in the CSS. If the example contradicts the docs, the docs are wrong.
