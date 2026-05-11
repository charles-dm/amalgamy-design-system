# Component: Input + Form Fields

## When to load this file
When building any form — job submission, policy creation, tenant onboarding,
search bars, or any screen with user-entered values.

## Figma source
Component sets: **Input** (72 variants) · **Select** (21 variants) · **Checkbox** (16) · **Radio** (12) · **Toggle** (12) · **Slider** (16)
File: R5IJ7h4lSqJq6smVlswkLQ · Page: 🧩 Components

---

## Input

### Variants
| Property | Options |
|---|---|
| Type | `text`, `number`, `search`, `password` |
| Size | `md` (45px), `sm` (36px), `xs` (28px) |
| State | `default`, `hover`, `focus`, `filled`, `error`, `disabled` |

### HTML anatomy

```html
<!-- Standard text input with label -->
<div class="field">
  <label class="field__label type-heading-xs" for="job-name">
    Job name
  </label>
  <input
    class="input"
    type="text"
    id="job-name"
    name="job-name"
    placeholder="e.g. bert-finetune-run-3"
    autocomplete="off"
  >
</div>

<!-- With help text -->
<div class="field">
  <label class="field__label type-heading-xs" for="job-name">
    Job name
  </label>
  <input class="input" type="text" id="job-name"
    placeholder="e.g. bert-finetune-run-3"
    aria-describedby="job-name-hint">
  <p class="field__hint type-body-xs text-secondary" id="job-name-hint">
    Letters, numbers, and hyphens only. Max 64 characters.
  </p>
</div>

<!-- Error state -->
<div class="field field--error">
  <label class="field__label type-heading-xs" for="job-name">
    Job name
  </label>
  <input class="input input--error" type="text" id="job-name"
    aria-describedby="job-name-error" aria-invalid="true"
    value="invalid name!!">
  <p class="field__error type-body-xs" id="job-name-error" role="alert">
    Only letters, numbers, and hyphens are allowed.
  </p>
</div>

<!-- Number input -->
<div class="field">
  <label class="field__label type-heading-xs" for="gpu-count">GPU count</label>
  <input class="input input--number" type="number"
    id="gpu-count" name="gpu-count"
    min="1" max="8" step="1" value="4">
</div>

<!-- Search -->
<div class="field field--search">
  <svg class="field__search-icon" aria-hidden="true"><!-- search icon --></svg>
  <input class="input input--search" type="search"
    placeholder="Search nodes..." aria-label="Search nodes">
</div>

<!-- Small -->
<div class="field">
  <label class="field__label type-heading-xs" for="priority">Priority</label>
  <input class="input input--sm" type="text" id="priority">
</div>

<!-- Disabled -->
<div class="field">
  <label class="field__label type-heading-xs" for="cluster-id">Cluster ID</label>
  <input class="input" type="text" id="cluster-id" disabled value="txtech-gpu-01">
</div>
```

### CSS

```css
/* Field wrapper */
.field {
  display:        flex;
  flex-direction: column;
  gap:            var(--space-1);
}
.field__label {
  font-family:  var(--font-sans);
  font-size:    var(--text-12);
  font-weight:  var(--weight-medium);
  color:        var(--color-text-secondary);
}
.field__hint {
  font-size: var(--text-12);
  color:     var(--color-text-tertiary);
  margin:    0;
}
.field__error {
  font-size: var(--text-12);
  color:     var(--color-danger-text);
  margin:    0;
}

/* Input */
.input {
  height:               45px;
  padding:              0 var(--space-3);
  font-family:          var(--font-sans);
  font-size:            var(--text-14);
  font-weight:          var(--weight-regular);
  color:                var(--color-text-primary);
  background:           var(--color-surface-1);
  border:               1px solid var(--color-border-default);
  border-radius:        var(--radius-md);
  outline:              none;
  width:                100%;
  font-variant-numeric: tabular-nums;
  transition:           border-color var(--duration-base) var(--ease-out),
                        box-shadow   var(--duration-base) var(--ease-out);
}
.input::placeholder { color: var(--color-text-tertiary); }
.input:hover        { border-color: var(--color-border-strong); }
.input:focus        {
  border-color: var(--color-accent);
  box-shadow:   0 0 0 3px var(--color-accent-subtle);
}
.input:disabled     {
  opacity: 0.5;
  cursor:  not-allowed;
  background: var(--color-surface-2);
}

/* Sizes */
.input--sm { height: 36px; font-size: var(--text-12); }
.input--xs { height: 28px; font-size: var(--text-11); padding: 0 var(--space-2); }

/* Error */
.input--error {
  border-color: var(--color-danger);
}
.input--error:focus {
  border-color: var(--color-danger);
  box-shadow:   0 0 0 3px var(--color-danger-bg);
}

/* Number — remove browser spinners */
.input--number::-webkit-inner-spin-button,
.input--number::-webkit-outer-spin-button { -webkit-appearance: none; }
.input--number { -moz-appearance: textfield; text-align: right; }

/* Search */
.field--search { position: relative; }
.field__search-icon {
  position:  absolute;
  left:      var(--space-3);
  top:       50%;
  transform: translateY(-50%);
  width:     16px;
  height:    16px;
  color:     var(--color-text-tertiary);
  pointer-events: none;
}
.input--search { padding-left: var(--space-7); }
```

---

## Select

### HTML anatomy

```html
<div class="field">
  <label class="field__label type-heading-xs" for="cluster-region">
    Cluster region
  </label>
  <div class="select-wrapper">
    <select class="select" id="cluster-region" name="cluster-region">
      <option value="">Select a region...</option>
      <option value="us-east">US East</option>
      <option value="us-west">US West</option>
      <option value="eu-central">EU Central</option>
    </select>
    <svg class="select-chevron" aria-hidden="true"><!-- chevron-down --></svg>
  </div>
</div>
```

### CSS

```css
.select-wrapper {
  position: relative;
  width:    100%;
}
.select {
  height:       44px;
  padding:      0 var(--space-7) 0 var(--space-3);
  width:        100%;
  font-family:  var(--font-sans);
  font-size:    var(--text-14);
  color:        var(--color-text-primary);
  background:   var(--color-surface-1);
  border:       1px solid var(--color-border-default);
  border-radius: var(--radius-md);
  outline:      none;
  appearance:   none;
  cursor:       pointer;
  transition:   border-color var(--duration-base) var(--ease-out);
}
.select:hover  { border-color: var(--color-border-strong); }
.select:focus  { border-color: var(--color-accent); box-shadow: 0 0 0 3px var(--color-accent-subtle); }
.select:disabled { opacity: 0.5; cursor: not-allowed; }
.select-chevron {
  position:       absolute;
  right:          var(--space-3);
  top:            50%;
  transform:      translateY(-50%);
  width:          16px;
  height:         16px;
  color:          var(--color-text-tertiary);
  pointer-events: none;
}
```

---

## Checkbox

Use for multi-select, table row selection, batch-save settings.

### HTML anatomy

```html
<!-- Standalone -->
<label class="checkbox-field">
  <input class="checkbox" type="checkbox" id="auto-retry" name="auto-retry">
  <span class="checkbox__box" aria-hidden="true"></span>
  <span class="checkbox__label type-body-sm">Auto-retry on failure</span>
</label>

<!-- Group -->
<fieldset class="field-group">
  <legend class="field-group__legend type-heading-xs">GPU types</legend>
  <div class="field-group__items">
    <label class="checkbox-field">
      <input class="checkbox" type="checkbox" name="gpu" value="a100" checked>
      <span class="checkbox__box" aria-hidden="true"></span>
      <span class="checkbox__label type-body-sm">A100</span>
    </label>
    <label class="checkbox-field">
      <input class="checkbox" type="checkbox" name="gpu" value="h100">
      <span class="checkbox__box" aria-hidden="true"></span>
      <span class="checkbox__label type-body-sm">H100</span>
    </label>
  </div>
</fieldset>

<!-- Table row selection -->
<td class="col-select">
  <label class="checkbox-field checkbox-field--no-label">
    <input class="checkbox checkbox--sm" type="checkbox"
      aria-label="Select node a100-node-07">
    <span class="checkbox__box" aria-hidden="true"></span>
  </label>
</td>
```

---

## Radio

Use for mutually exclusive choices with 2–5 visible options.

### HTML anatomy

```html
<fieldset class="field-group">
  <legend class="field-group__legend type-heading-xs">Job priority</legend>
  <div class="field-group__items">
    <label class="radio-field">
      <input class="radio" type="radio" name="priority" value="normal" checked>
      <span class="radio__dot" aria-hidden="true"></span>
      <span class="radio__label type-body-sm">Normal</span>
    </label>
    <label class="radio-field">
      <input class="radio" type="radio" name="priority" value="high">
      <span class="radio__dot" aria-hidden="true"></span>
      <span class="radio__label type-body-sm">High</span>
    </label>
    <label class="radio-field">
      <input class="radio" type="radio" name="priority" value="critical">
      <span class="radio__dot" aria-hidden="true"></span>
      <span class="radio__label type-body-sm">Critical</span>
    </label>
  </div>
</fieldset>
```

---

## Toggle

Use for binary settings that take immediate effect (no save required).

### HTML anatomy

```html
<label class="toggle-field">
  <input class="toggle__input" type="checkbox" role="switch"
    id="notifications" name="notifications" checked
    aria-checked="true">
  <span class="toggle__track" aria-hidden="true">
    <span class="toggle__thumb"></span>
  </span>
  <span class="toggle__label type-body-sm">Email notifications</span>
</label>
```

---

## Form layout

```html
<!-- Single-column form (Template D) -->
<form class="form" novalidate>
  <div class="form__section">
    <h2 class="type-heading-section">Job configuration</h2>
    <div class="form__grid">
      <div class="field"><!-- Job name --></div>
      <div class="field"><!-- GPU count --></div>
      <div class="field"><!-- Priority --></div>
    </div>
  </div>

  <div class="form__section">
    <h2 class="type-heading-section">Resources</h2>
    <div class="form__grid">
      <div class="field"><!-- Cluster --></div>
      <div class="field"><!-- Memory --></div>
    </div>
  </div>

  <div class="form__actions">
    <button class="btn btn--secondary" type="button">Cancel</button>
    <button class="btn btn--primary" type="submit">Submit Job</button>
  </div>
</form>
```

```css
.form { display: flex; flex-direction: column; gap: var(--space-7); }
.form__section { display: flex; flex-direction: column; gap: var(--space-5); }
.form__grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: var(--space-4); }
.form__actions {
  display: flex; justify-content: flex-end; gap: var(--space-3);
  padding-top: var(--space-5);
  border-top: 1px solid var(--color-border-subtle);
}
.field-group { border: none; padding: 0; margin: 0; }
.field-group__legend { margin-bottom: var(--space-3); }
.field-group__items { display: flex; flex-direction: column; gap: var(--space-2); }
.checkbox-field, .radio-field, .toggle-field {
  display: flex; align-items: center; gap: var(--space-2); cursor: pointer;
}
```

---

## Rules

- **Every input has a visible `<label>`.** Never placeholder-only. Placeholders disappear on focus.
- **Use `<fieldset>` + `<legend>` for checkbox/radio groups.** Never just a `<div>`.
- **Toggle = immediate effect only.** If the setting requires a Save button, use a checkbox instead.
- **Select for 5–15 options.** Fewer: radio buttons. More: searchable combobox.
- **Never use `type="number"` for IDs or codes.** Use `type="text"` with `inputmode="numeric"`.
- **Error messages below the input, not above.** Use `aria-describedby` to associate them.
- **`novalidate` on the `<form>`.** Handle validation in JS — browser native validation is not styled.
