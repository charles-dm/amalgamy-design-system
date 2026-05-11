# Component: Icons

## When to load this file
When a screen needs visual glyphs for status, directional affordances,
brand sigils, or any UI element where a small mark carries meaning the
surrounding text cannot. Also: icon-only buttons, sidebar nav items,
empty-state illustrations, alert markers.

## Library
**Phosphor Icons** ([phosphoricons.com](https://phosphoricons.com)) — the only icon
library in the system. Loaded via CDN; no other icon set may be used.

```html
<script src="https://unpkg.com/@phosphor-icons/web"></script>
```

Browse and search the full catalog at [phosphoricons.com](https://phosphoricons.com).
Click any icon to copy its class name.

---

## Weights

Phosphor ships six weights. Use them deliberately — they carry different voices.

| Weight | Class | Use |
|---|---|---|
| Regular | `ph` (alias for `ph-regular`) | **Default.** Inline with body text, nav items, secondary affordances. |
| Bold    | `ph-bold`     | Emphasis. Active states. Primary action affordances. |
| Fill    | `ph-fill`     | Status indicators (filled circles, badges). Selected states. |
| Duotone | `ph-duotone`  | Decorative or marketing contexts. Avoid in dense operator UI. |
| Light   | `ph-light`    | Rarely. Quietest weight. |
| Thin    | `ph-thin`     | Almost never — fails contrast at small sizes. |

Most prototypes ship with **regular for inline use** and **bold or fill for status**. Mixing too many weights on one screen reads as noise.

---

## HTML anatomy

```html
<!-- Inline with text -->
<span>Submitted <i class="ph ph-check-circle"></i></span>

<!-- Status indicator -->
<i class="ph-fill ph-circle icon-success"></i> Healthy
<i class="ph-fill ph-circle icon-warning"></i> Throttled
<i class="ph-fill ph-circle icon-danger"></i>  Failed

<!-- Icon-only button -->
<button class="btn btn--icon-only" aria-label="Close">
  <i class="ph ph-x"></i>
</button>

<!-- Sidebar nav item -->
<a class="sidebar-item">
  <i class="ph ph-list-bullets"></i>
  <span>Jobs</span>
</a>

<!-- Sized variant -->
<i class="ph ph-terminal icon-lg"></i>
```

---

## Sizing

Icons are font-based — they inherit `font-size` from their parent.
Use the size tokens defined in `amalgamy-reset.css`:

| Class | Size | Pairs with |
|---|---|---|
| `.icon-sm` | 12px | `.type-heading-xs`, `.type-caps`, kbd hints |
| (default)  | 14px | `.type-body-sm`, button labels, table cells |
| `.icon-lg` | 16px | `.type-body`, page headers |
| `.icon-xl` | 20px | `.type-heading-group`, section titles, empty-state illustrations |

Never hardcode `font-size` on an icon. Use a token or accept inheritance.

---

## Color

Icons inherit `currentColor` from the parent. Three ways to color them:

1. **Inherit from parent text color** — preferred. The icon adopts whatever the surrounding text uses.
2. **Accent classes** — `.icon-accent`, `.icon-aqua`, `.icon-subtle`, `.icon-faint`, `.icon-success`, `.icon-warning`, `.icon-danger`, `.icon-info`. Apply on the icon element directly.
3. **Inline `color: var(--token)`** — last resort. Only when the surrounding context shouldn't carry the icon's color.

```html
<!-- Inherits text color -->
<span style="color: var(--color-text-secondary)">
  <i class="ph ph-clock"></i> 14:32 UTC
</span>

<!-- Accent class on icon -->
<i class="ph-fill ph-circle icon-success"></i>

<!-- Inside a styled parent -->
<a class="sidebar-item is-active">
  <i class="ph ph-list-bullets"></i>  <!-- adopts active state color -->
  <span>Jobs</span>
</a>
```

---

## When to use which weight

| Context | Weight |
|---|---|
| Inline with body text | `ph` (regular) |
| Sidebar nav item — inactive | `ph` (regular) |
| Sidebar nav item — active | `ph-bold` or `ph-fill` |
| Status badge (●) | `ph-fill ph-circle` |
| Status indicator inline | `ph-fill ph-circle icon-sm` |
| Button leading/trailing icon | `ph` (regular) — match button label weight |
| Icon-only button | `ph-bold` — needs more weight to anchor |
| Empty-state illustration | `ph-duotone` at `.icon-xl` or larger |
| Alert/feedback marker | `ph-fill` (with status accent class) |
| Chevron, arrow, caret | `ph` (regular) — directional, doesn't need emphasis |

---

## Conventions

- **One library only.** Phosphor everywhere; never mix with Lucide, Heroicons, Feather, or system glyphs. The visual rhythm depends on a single stroke-weight family.
- **Use sparsely.** The system is text-led. Most screens should carry fewer than ten icons. If a screen looks icon-heavy, it has stopped being Amalgamy.
- **Earn the icon.** It must add information the surrounding text can't carry — status, direction, brand sigil. Decorating headings or prefixing every nav item is icon noise.
- **Aria-label icon-only buttons.** Phosphor icons are presentational; they don't ship with semantics. Anywhere an icon stands alone, the parent control must carry the accessible name.
- **Don't override stroke width.** The Phosphor weights are the system's stroke language. Inventing CSS to thin or thicken a weight breaks the rhythm.
- **Don't mix weights at the same scale.** A row of icons should all be the same weight. Different weights signal different states (active vs. inactive, filled vs. outline) — not different categories.

---

## Common mistakes

| Mistake | Fix |
|---|---|
| Icon hardcoded to 14px or 16px | Use `font-size: var(--icon-size)` or the `.icon-{sm,lg,xl}` classes |
| `color: #2ECEC0` on an icon | Use `.icon-accent` or set color on the parent |
| Icon-only button with no aria-label | Add `aria-label="Close"` (or equivalent) to the button |
| Multiple icon weights in one row | Pick one weight for the row; weight = state, not category |
| Icon doesn't visually align with text | Adjust `vertical-align` on the icon (default is `-0.15em`) — but first verify the parent line-height isn't wrong |
| Lucide / Feather / Heroicons appearing in a Phosphor page | Replace with the equivalent Phosphor icon. The stroke families don't blend. |

---

## Migration note

The system previously used Lucide (v0.1, briefly). All Lucide references should be replaced with Phosphor equivalents. Lucide and Phosphor share most icon names but not stroke families — never run both on the same page.
