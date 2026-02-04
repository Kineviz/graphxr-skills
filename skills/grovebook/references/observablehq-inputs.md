# Observable Inputs Reference

> Lightweight user interface components for exploring data and building interactive displays.

## Button

Do something when a button is clicked.

```js
Inputs.button(content, options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `required` | boolean | `false` | If true, initial value defaults to undefined |
| `value` | any | `0` or `null` | The initial value |
| `reduce` | function | `v => v + 1` | Function to update value on click |
| `width` | string | - | Width of the input (not including label) |
| `disabled` | boolean | `false` | Whether input is disabled |

```js
// Simple button (value = click count)
viewof clicks = Inputs.button("OK", {label: "Click me"})

// Custom reduce function
viewof time = Inputs.button("Refresh", {value: null, reduce: () => Date.now()})

// Multiple buttons
viewof counter = Inputs.button([
  ["Increment", value => value + 1],
  ["Decrement", value => value - 1],
  ["Reset", () => 0]
], {label: "Counter", value: 0})
```

---

## Toggle

Toggle between two values (on or off).

```js
Inputs.toggle(options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `values` | Array | `[true, false]` | The two values to toggle between |
| `value` | any | Second value (`false`) | The initial value |
| `disabled` | boolean | `false` | Whether input is disabled |

```js
viewof mute = Inputs.toggle({label: "Mute"})
viewof mode = Inputs.toggle({label: "Mode", values: ["light", "dark"]})
```

---

## Checkbox

Choose any from a set (multiple selection).

```js
Inputs.checkbox(data, options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `sort` | boolean \| string \| function | `false` | Sort keys: `true`, `"ascending"`, `"descending"`, or comparator |
| `unique` | boolean | `false` | Only show unique keys |
| `locale` | string | English | Current locale |
| `format` | function | `formatLocaleAuto` | Format function for display |
| `keyof` | function | `d => d` | Function to return key for element |
| `valueof` | function | `d => d` | Function to return value of element |
| `value` | Array | `[]` | Initial selected values |
| `disabled` | boolean \| Array | `false` | Disable all or specific values |

```js
viewof flavors = Inputs.checkbox(
  ["salty", "sweet", "bitter", "sour", "umami"], 
  {label: "Flavors"}
)

// With grouped data
viewof sports = Inputs.checkbox(d3.group(athletes, d => d.sport))
```

---

## Radio

Choose one from a set (single selection).

```js
Inputs.radio(data, options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `sort` | boolean \| string \| function | `false` | Sort keys |
| `unique` | boolean | `false` | Only show unique keys |
| `locale` | string | English | Current locale |
| `format` | function | `formatLocaleAuto` | Format function for display |
| `keyof` | function | `d => d` | Function to return key for element |
| `valueof` | function | `d => d` | Function to return value of element |
| `value` | any | `null` | Initial selected value |
| `disabled` | boolean \| Array | `false` | Disable all or specific values |

```js
viewof flavor = Inputs.radio(
  ["salty", "sweet", "bitter", "sour", "umami"], 
  {label: "Flavor"}
)

viewof color = Inputs.radio(["red", "green", "blue"], {label: "Color"})
```

---

## Range

Pick a number using a slider.

```js
Inputs.range(extent, options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `step` | number | - | Step precision between values |
| `format` | function | `formatTrim` | Number format function |
| `placeholder` | string | - | Placeholder when empty |
| `transform` | function | - | Non-linear transform function |
| `invert` | function | - | Inverse of transform |
| `validate` | function | `checkValidity` | Validation function |
| `value` | number | `(min + max) / 2` | Initial value |
| `width` | string | - | Width of input |
| `disabled` | boolean | `false` | Whether disabled |

```js
viewof gain = Inputs.range([0, 11], {value: 5, step: 0.1, label: "Gain"})
viewof n = Inputs.range([0, 255], {step: 1, label: "Favorite number"})

// Logarithmic scale
viewof x = Inputs.range([1, 1000], {transform: Math.log, label: "Log scale"})
```

**Transform notes:** Passing `Math.sqrt`, `Math.log`, or `Math.exp` will auto-supply the inverse.

---

## Number

Equivalent to Range but only shows a number input (no slider).

```js
Inputs.number(options)
Inputs.number(extent, options)
```

If only options specified, extent defaults to `[-Infinity, Infinity]`.

---

## Select

Choose one or many from a dropdown menu.

```js
Inputs.select(data, options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `multiple` | boolean | `false` | Allow multiple selections |
| `size` | number | - | Visible options (if multiple) |
| `sort` | boolean \| string \| function | `false` | Sort keys |
| `unique` | boolean | `false` | Only show unique keys |
| `locale` | string | English | Current locale |
| `format` | function | `formatLocaleAuto` | Format function (must return string) |
| `keyof` | function | `d => d` | Function to return key |
| `valueof` | function | `d => d` | Function to return value |
| `value` | any \| Array | - | Initial value |
| `width` | string | - | Width of input |
| `disabled` | boolean \| Array | `false` | Disable all or specific values |

```js
viewof size = Inputs.select(["Small", "Medium", "Large"], {label: "Size"})

viewof homeState = Inputs.select([null].concat(stateNames), {label: "Home state"})

viewof inks = Inputs.select(
  ["cyan", "magenta", "yellow", "black"], 
  {multiple: true, label: "Inks"}
)
```

---

## Text

Freeform single-line text input.

```js
Inputs.text(options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `type` | string | `"text"` | Input type: `"password"`, `"email"`, etc. |
| `value` | string | `""` | Initial value |
| `placeholder` | string | - | Placeholder text |
| `spellcheck` | boolean | - | Enable spell-checker |
| `autocomplete` | string \| boolean | - | Autocomplete attribute |
| `autocapitalize` | string \| boolean | - | Autocapitalize attribute |
| `pattern` | string | - | Regex pattern for validation |
| `minlength` | number | - | Minimum length |
| `maxlength` | number | - | Maximum length |
| `min` | string | - | Minimum value (for certain types) |
| `max` | string | - | Maximum value (for certain types) |
| `required` | boolean | `minlength > 0` | Must be non-empty |
| `validate` | function | `checkValidity` | Custom validation |
| `width` | string | - | Width of input |
| `submit` | boolean | `false` | Require explicit submission |
| `datalist` | Iterable | - | Suggested values |
| `readonly` | boolean | `false` | Read-only state |
| `disabled` | boolean | `false` | Disabled state |

```js
viewof name = Inputs.text({label: "Name", placeholder: "What's your name?"})

viewof query = Inputs.text({
  label: "Search",
  placeholder: "Enter query...",
  submit: true
})
```

---

## Email

Text input with `type="email"`.

```js
Inputs.email(options)
```

---

## Tel

Text input with `type="tel"`.

```js
Inputs.tel(options)
```

---

## URL

Text input with `type="url"`.

```js
Inputs.url(options)
```

---

## Password

Text input with `type="password"`.

```js
Inputs.password(options)
```

---

## Textarea

Multi-line freeform text input.

```js
Inputs.textarea(options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `value` | string | `""` | Initial value |
| `placeholder` | string | - | Placeholder text |
| `spellcheck` | boolean | - | Enable spell-checker |
| `autocomplete` | string \| boolean | - | Autocomplete attribute |
| `autocapitalize` | string \| boolean | - | Autocapitalize attribute |
| `minlength` | number | - | Minimum length |
| `maxlength` | number | - | Maximum length |
| `required` | boolean | `minlength > 0` | Must be non-empty |
| `validate` | function | `checkValidity` | Custom validation |
| `width` | string | - | Width of input |
| `rows` | number | - | Number of visible rows |
| `resize` | boolean | `rows < 12` | Allow vertical resizing |
| `submit` | boolean | `false` | Require explicit submission |
| `readonly` | boolean | `false` | Read-only state |
| `disabled` | boolean | `false` | Disabled state |
| `monospace` | boolean | `false` | Use monospace font |

```js
viewof bio = Inputs.textarea({
  label: "Biography", 
  placeholder: "Tell us a little about yourself…",
  rows: 6
})
```

---

## Date

Calendar-based date input.

```js
Inputs.date(options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `value` | Date \| string | `null` | Initial value (Date or ISO yyyy-mm-dd) |
| `min` | string | - | Minimum date |
| `max` | string | - | Maximum date |
| `required` | boolean | - | Must be valid date |
| `validate` | function | - | Custom validation |
| `width` | string | - | Width of input |
| `submit` | boolean | `false` | Require explicit submission |
| `readonly` | boolean | `false` | Read-only state |
| `disabled` | boolean | `false` | Disabled state |

**Value:** Returns a `Date` instance at UTC midnight, or `null` if invalid/empty.

```js
viewof birthday = Inputs.date({label: "Birthday"})
viewof start = Inputs.date({label: "Start date", value: "1982-03-06"})
```

---

## Datetime

Like Date but includes time selection (in user's local time zone).

```js
Inputs.datetime(options)
```

```js
viewof start = Inputs.datetime({label: "Start date", value: "1982-03-06T02:30"})
```

---

## Color

Color picker input.

```js
Inputs.color(options)
```

**Value:** RGB hexadecimal string (e.g., `#ff00ff`).

```js
viewof color = Inputs.color({label: "Favorite color", value: "#4682b4"})
```

**Note:** Does not support: `placeholder`, `pattern`, `spellcheck`, `autocomplete`, `autocapitalize`, `min`, `max`, `minlength`, `maxlength`.

---

## File

Local file input.

```js
Inputs.file(options)
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `label` | string \| HTMLElement | - | A label for the input |
| `required` | boolean | - | Must select a valid file |
| `validate` | function | - | Custom validation |
| `accept` | string | - | Acceptable file types (e.g., `".csv"`, `"image/*"`) |
| `capture` | string | - | For capturing image/video |
| `multiple` | boolean | `false` | Allow multiple files |
| `width` | string | - | Width of input |
| `disabled` | boolean | `false` | Disabled state |

**Value:** Files are exposed with the same API as Observable file attachments.

**Note:** File input value cannot be set programmatically.

```js
viewof file = Inputs.file({label: "CSV file", accept: ".csv", required: true})
```

---

## Quick Reference

```js
Inputs.button(content, options)      // Click trigger
Inputs.toggle(options)               // On/off switch
Inputs.checkbox(data, options)       // Multiple selection
Inputs.radio(data, options)          // Single selection (visible)
Inputs.range(extent, options)        // Number slider
Inputs.number(extent?, options)      // Number input only
Inputs.select(data, options)         // Dropdown menu
Inputs.text(options)                 // Single-line text
Inputs.email(options)                // Email input
Inputs.tel(options)                  // Telephone input
Inputs.url(options)                  // URL input
Inputs.password(options)             // Password input
Inputs.textarea(options)             // Multi-line text
Inputs.date(options)                 // Date picker
Inputs.datetime(options)             // Date + time picker
Inputs.color(options)                // Color picker
Inputs.file(options)                 // File upload
```

### Common Options

| Option | Description |
|--------|-------------|
| `label` | Label string or HTML element |
| `value` | Initial value |
| `disabled` | Disable interaction |
| `width` | Input width (not including label) |

---

*Generated from [Observable Inputs documentation](https://observablehq.com/documentation/inputs/overview) and [GitHub repository](https://github.com/observablehq/inputs).*