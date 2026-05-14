# Component: Button

## When to load this file
When building any screen that contains an interactive action — form submission,
confirmation, navigation trigger, or any call to action.

## Figma source
Component set: **Button**
Variants: Variant × Size × Pattern × State = 120 combinations
File: R5IJ7h4lSqJq6smVlswkLQ · Page: 🧩 Components

---

## Variants

### Variant (visual style)
| Value | Use |
|---|---|
| `primary` | Single most important action per section. Teal fill (#2ECEC0). One per screen section. |
| `outlined` | Low-emphasis actions that need a border — View Details, Learn More |
| `filled` | Alternative surface fill — tab bars, toolbars, non-CTA contexts |
| `destructive` | Irreversible actions — Delete, Remove, Revoke. Always confirm first. |

> **Note:** The `secondary` (aqua) variant was removed in May 2026.
> `primary` is now teal (#2ECEC0). If you have instances using
> `variant=secondary`, update them to `variant=primary`.

### Size
| Value | Height | Padding | Use |
|---|---|---|---|
| `md` | 40px | `paddingTop: 13px, paddingBottom: 12px` | Default. Page headers, modal footers, form submit |
| `sm` | 32px | `paddingTop: 7px, paddingBottom: 7px` | Toolbar actions, table row inline actions, dense contexts |

> **Note:** `md` is 40px via asymmetric padding (13+12) because text renders
> at 15px line-height. Symmetric 12+12 gives 39px — visually incorrect.

### Pattern
| Value | Use |
|---|---|
| `label` | Text only. Default. |
| `label-icon` | Icon left or right of label. Leading icon adds meaning; trailing icon signals behavior. |
| `icon-only` | No text label. Only use when the icon is universally understood (✕ close, + add). Always add `aria-label`. |

### Icon instances in buttons

The Icon component (2675:8397) is placed as a live instance inside all
`label-icon` and `icon-only` button variants via `INSTANCE_SWAP` property.

**Icon sizing per button size:**
- `md` button → `Icon Size=sm` (16×16px)
- `sm` button → `Icon Size=xs` (12×12px)

**Icon color per button variant:**
- `primary` (teal bg) → `Color=inverse` — dark #051315 icon on teal fill
- `destructive` (red bg) → `Color=inverse` — same logic
- `outlined` → `Color=default` — light #EAF1EC icon on dark bg
- `filled` → `Color=default`

**Weight:** Light throughout — matches the terminal/developer aesthetic.

**Color=inverse note:** The inverse icon variants had a background fill bug
(dark #051315 frame fill). Fixed in May 2026 — fills cleared on all 8
inverse variants. The icon color comes from strokes only, not fills.

**INSTANCE_SWAP property:** The `icon` property on the Button component set
lets designers swap which specific icon appears from the right panel without
detaching the button instance. Default icon: ArrowsClockwise (Light).

### State
Managed by CSS — do not set manually in HTML. Interactive states are:
`default` → `hover` → `active` → `disabled`

---

## HTML anatomy

### Primary (default)
```html
<button class="btn btn--primary" type="button">
  Submit Job
</button>
```

### With leading icon
```html
<button class="btn btn--primary btn--icon-leading" type="button">
  <svg class="btn__icon" aria-hidden="true"><!-- icon --></svg>
  Submit Job
</button>
```

### With trailing icon
```html
<button class="btn btn--secondary btn--icon-trailing" type="button">
  Export
  <svg class="btn__icon" aria-hidden="true"><!-- chevron-right --></svg>
</button>
```

### Icon only
```html
<button class="btn btn--outlined btn--icon-only" type="button" aria-label="Add node">
  <svg class="btn__icon" aria-hidden="true"><!-- plus --></svg>
</button>
```

### Small size
```html
<button class="btn btn--secondary btn--sm" type="button">
  View Details
</button>
```

### Destructive
```html
<button class="btn btn--destructive" type="button">
  Delete Cluster
</button>
```

### Disabled
```html
<button class="btn btn--primary" type="button" disabled>
  Submit Job
</button>
```

---

## CSS

```css
.btn {
  display:         inline-flex;
  align-items:     center;
  justify-content: center;
  gap:             var(--space-2);
  height:          40px;
  padding:         0 var(--space-4);
  font-family:     var(--font-mono);
  font-size:       var(--text-14);
  font-weight:     var(--weight-bold);
  letter-spacing:  var(--tracking-wide);
  text-transform:  uppercase;
  border-radius:   var(--radius-md);
  border:          1px solid transparent;
  white-space:     nowrap;
  cursor:          pointer;
  transition:      background var(--duration-base) var(--ease-out),
                   border-color var(--duration-base) var(--ease-out),
                   color var(--duration-base) var(--ease-out),
                   opacity var(--duration-base) var(--ease-out);
}

/* Size */
.btn--sm {
  height:    32px;
  padding:   0 var(--space-3);
  font-size: var(--text-12);
}

/* Icon helpers */
.btn__icon {
  width:      16px;
  height:     16px;
  flex-shrink: 0;
}
.btn--icon-only {
  padding: 0;
  width:   40px;
}
.btn--icon-only.btn--sm {
  width: 32px;
}

/* Variants — dark mode */
.btn--primary {
  background:  var(--color-accent);
  color:       var(--color-text-inverse);
  border-color: var(--color-accent);
}
.btn--primary:hover  { background: var(--color-accent-hover); border-color: var(--color-accent-hover); }
.btn--primary:active { background: var(--color-accent-active); border-color: var(--color-accent-active); }

.btn--secondary {
  background:  transparent;
  color:       var(--color-text-primary);
  border-color: var(--color-border-default);
}
.btn--secondary:hover  { background: var(--color-surface-2); }
.btn--secondary:active { background: var(--color-surface-3); }

.btn--outlined {
  background:  transparent;
  color:       var(--color-accent);
  border-color: var(--color-accent);
}
.btn--outlined:hover  { background: var(--color-accent-subtle); }

.btn--filled {
  background:  var(--color-surface-3);
  color:       var(--color-text-primary);
  border-color: var(--color-border-default);
}
.btn--filled:hover  { background: var(--color-surface-2); }

.btn--destructive {
  background:  var(--color-danger);
  color:       #fff;
  border-color: var(--color-danger);
}
.btn--destructive:hover  { background: var(--color-danger-hover); border-color: var(--color-danger-hover); }
.btn--destructive:active { background: var(--color-danger-active); border-color: var(--color-danger-active); }

/* Disabled (all variants) */
.btn:disabled,
.btn[aria-disabled="true"] {
  opacity: 0.4;
  cursor:  not-allowed;
  pointer-events: none;
}
```

---

## Rules

- **One primary per section.** If you need two primary buttons, one of them is wrong.
- **Never use for navigation.** Use `<a>` elements for page transitions.
- **Never disable without a reason.** If you can't explain why it's disabled inline, keep it enabled and show an error on attempt.
- **Destructive always confirms.** Never let `.btn--destructive` execute immediately — require a confirmation dialog or explicit second click.
- **Icon-only always has `aria-label`.** No exceptions.
- **Label text is UPPERCASE in Figma but set via CSS `text-transform: uppercase`** — write labels in sentence case in HTML.

---

## Do not do

```html
<!-- ✗ Two primaries -->
<button class="btn btn--primary">Submit Job</button>
<button class="btn btn--primary">Save Draft</button>

<!-- ✗ Button for navigation -->
<button class="btn btn--secondary" onclick="window.location='/jobs'">View Jobs</button>

<!-- ✗ Disabled with no context -->
<button class="btn btn--primary" disabled>Submit Job</button>

<!-- ✓ Correct: explain why -->
<button class="btn btn--primary" disabled aria-describedby="submit-hint">Submit Job</button>
<p id="submit-hint" class="type-body-xs text-secondary">Select a cluster to enable submission</p>
```
