# CSS Flexbox — Complete Reference Notes
## Source: CSS-Tricks Complete Guide to Flexbox

Flexbox is a layout system designed to distribute space and align items
in a container efficiently, even when sizes are unknown or dynamic. It
works in one direction at a time (either a row or a column), making it
ideal for components and small-scale layouts. For large page-level grid
layouts, CSS Grid is more appropriate.

---

## Core Terminology

The flex layout is based on two axes:

- **Main axis** — the primary axis items are laid out along. Its direction
  is set by `flex-direction` (horizontal by default, vertical with `column`)
- **Cross axis** — the axis perpendicular to the main axis
- **Flex container** — the parent element with `display: flex`
- **Flex items** — the direct children of the flex container

---

## Container Properties (applied to the parent)

### display
Activates flexbox on the container. All direct children become flex items.
```css
.container {
  display: flex; /* or inline-flex */
}
```

### flex-direction
Sets the main axis — determines which direction items flow.
```css
.container {
  flex-direction: row;           /* default — left to right */
  flex-direction: row-reverse;   /* right to left */
  flex-direction: column;        /* top to bottom */
  flex-direction: column-reverse; /* bottom to top */
}
```

### flex-wrap
By default items try to fit on one line. `flex-wrap` lets them wrap.
```css
.container {
  flex-wrap: nowrap;        /* default — everything on one line */
  flex-wrap: wrap;          /* wraps onto multiple lines top to bottom */
  flex-wrap: wrap-reverse;  /* wraps onto multiple lines bottom to top */
}
```

### flex-flow
Shorthand for `flex-direction` and `flex-wrap` combined. Default: `row nowrap`.
```css
.container {
  flex-flow: column wrap;
}
```

### justify-content
Controls alignment along the **main axis**. Distributes extra space.
```css
.container {
  justify-content: flex-start;    /* default — items at start */
  justify-content: flex-end;      /* items at end */
  justify-content: center;        /* items centered */
  justify-content: space-between; /* first item at start, last at end, even gaps between */
  justify-content: space-around;  /* equal space around each item (half-size edges) */
  justify-content: space-evenly;  /* equal space between items AND edges */
}
```
The safest values for browser support are `flex-start`, `flex-end`, and `center`.

### align-items
Controls alignment along the **cross axis** for items on a single line.
Think of it as `justify-content` but for the other axis.
```css
.container {
  align-items: stretch;     /* default — items stretch to fill container height */
  align-items: flex-start;  /* items aligned to start of cross axis */
  align-items: flex-end;    /* items aligned to end of cross axis */
  align-items: center;      /* items centered on cross axis */
  align-items: baseline;    /* items aligned by their text baselines */
}
```

### align-content
Controls alignment of **multiple lines** along the cross axis when items
wrap. Has no effect on single-line containers (`flex-wrap: nowrap`).
```css
.container {
  align-content: flex-start;    /* lines packed to start */
  align-content: flex-end;      /* lines packed to end */
  align-content: center;        /* lines centered */
  align-content: space-between; /* first line at start, last at end */
  align-content: space-around;  /* equal space around each line */
  align-content: stretch;       /* lines stretch to fill remaining space */
}
```

### gap
Controls space **between** flex items only — not on outer edges.
```css
.container {
  gap: 10px;          /* equal gap on all sides between items */
  gap: 10px 20px;     /* row-gap column-gap */
  row-gap: 10px;
  column-gap: 20px;
}
```

---

## Item Properties (applied to the children)

### order
Controls the visual order of items. Default is `0`. Items with higher
values appear later. Items with the same order follow source order.
```css
.item {
  order: 2; /* default 0 */
}
```
Note: order is visual only — screen readers still follow source order.

### flex-grow
Defines the ability for an item to grow and take up available space.
Acts as a proportion — if all items have `flex-grow: 1` they share space
equally. An item with `flex-grow: 2` takes twice as much as others.
```css
.item {
  flex-grow: 1; /* default 0 — items don't grow by default */
}
```

### flex-shrink
Defines the ability for an item to shrink when necessary.
```css
.item {
  flex-shrink: 1; /* default 1 — items can shrink */
}
```

### flex-basis
Sets the default size of an item before remaining space is distributed.
Think of it as the item's "ideal" or starting size.
```css
.item {
  flex-basis: 200px; /* or auto (default) */
}
```

### flex (shorthand — recommended)
Shorthand for `flex-grow`, `flex-shrink`, and `flex-basis`. The second
and third values are optional. Default is `0 1 auto`.
```css
.item {
  flex: 1;            /* grow: 1, shrink: 1, basis: 0% */
  flex: 1 200px;      /* grow: 1, basis: 200px */
  flex: 2 1 auto;     /* grow: 2, shrink: 1, basis: auto */
}
```
**It is recommended to use the shorthand** rather than set the individual
properties separately, as the shorthand sets other values intelligently.

### align-self
Overrides `align-items` for a single specific item.
```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```
Note: `float`, `clear`, and `vertical-align` have no effect on flex items.

---

## Common Patterns

### Perfect centering (the classic Flexbox party trick)
```css
.parent {
  display: flex;
  height: 300px;
}
.child {
  margin: auto; /* absorbs all extra space — centers on both axes */
}
```

### Responsive navigation — right-aligned on desktop, centered on medium, stacked on mobile
```css
.navigation {
  display: flex;
  flex-flow: row wrap;
  justify-content: flex-end;  /* desktop: right aligned */
}
@media (max-width: 800px) {
  .navigation { justify-content: space-around; } /* medium: centered */
}
@media (max-width: 500px) {
  .navigation { flex-direction: column; } /* mobile: stacked */
}
```

### Mobile-first responsive layout with flex
Start all items at full width on mobile, then use `flex` shorthand in
media queries to build the multi-column layout as the viewport grows:
```css
.wrapper { display: flex; flex-flow: row wrap; }
.wrapper > * { flex: 1 100%; } /* all full width by default */

@media (min-width: 600px) {
  .aside { flex: 1 auto; } /* sidebars share a row */
}
@media (min-width: 800px) {
  .main { flex: 3 0px; }   /* main takes 3x the space */
}
```