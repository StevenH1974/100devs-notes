# Shay Howe Advanced HTML & CSS — Lesson 1: Performance & Organization

## Strategy & Structure

Good performance starts before a single line of code is written. The way you 
organize your stylesheet architecture affects how fast your pages render and 
how maintainable your code is long-term. Shay recommends thinking of websites 
as systems rather than individual pages, and structuring your CSS to reflect that.

A common folder structure separates styles into three directories:
- **Base** — global defaults like normalize.css, layout.css, typography.css
- **Components** — reusable UI elements like buttons, forms, alerts, nav
- **Modules** — page-specific sections driven by business logic like header, 
  footer, sidebar

### OOCSS (Object Oriented CSS)
Pioneered by Nicole Sullivan. Built on two principles:
1. **Separate structure from skin** — keep layout separate from visual theme
2. **Separate content from container** — a heading should look the same 
   regardless of what element wraps it

### SMACSS (Scalable and Modular Architecture for CSS)
Developed by Jonathan Snook. Breaks styles into five categories:
1. **Base** — default element styles
2. **Layout** — sizing and grid
3. **Module** — specific page components like nav or features
4. **State** — overrides for alternate states (e.g. is-active, is-error)
5. **Theme** — visual skin/look and feel

You don't have to pick one methodology strictly — most developers borrow 
principles from both.

---

## Performance Driven Selectors

### Keep Selectors Short
Browsers read CSS selectors from right to left. A long selector like 
`header nav ul li a` forces the browser to work through every level. 
A simple class like `.primary-link` is far more efficient and easier to maintain.

### Favor Classes Over IDs and Element Selectors
- Classes render quickly, are reusable, and are flexible
- Avoid prefixing classes with element types (e.g. `article.feat-post` — bad; 
  `.feat-post` — good)
- Avoid ID selectors where possible — they are overly specific and 
  essentially behave like `!important`
- The **key selector** (the rightmost part of a selector) is what the browser 
  finds first — make it specific and meaningful

---

## Reusable Code

Repeating the same CSS declarations across multiple selectors bloats file size 
and hurts performance. Instead, group selectors that share styles using a comma, 
or create a shared base class that multiple elements inherit from.

```css
/* Bad — repeating styles */
.news { background: #eee; border-radius: 5px; }
.social { background: #eee; border-radius: 5px; }

/* Good — shared selector */
.news, .social { background: #eee; border-radius: 5px; }

/* Even Better — shared base class */
.modal { background: #eee; border-radius: 5px; }
```

---

## Minify & Compress Files

### gzip Compression
gzip is a server-side compression method that finds repeated strings in files 
(HTML, CSS, JS) and compresses them. It can reduce file sizes by around 60%. 
It is configured via an `.htaccess` file on Apache servers. The HTML5 Boilerplate 
project provides ready-made configs for this.

### Image Compression
Images can be compressed in a lossless way — removing unnecessary color profiles 
and metadata without reducing visual quality. Tools: ImageOptim (Mac), 
PNGGauntlet (Windows).

Setting explicit `height` and `width` attributes on images in HTML helps the 
browser reserve space while the page loads, improving render speed. However, 
never use these attributes to scale down a large image — always use the actual 
display size.

---

## Reduce HTTP Requests

Every file the browser has to request from the server adds load time. Fewer 
requests = faster pages.

### Combine Files
Combine all CSS into one file and all JavaScript into one file before deploying.
- CSS should load in the `<head>` so it renders alongside the page
- JavaScript should load just before the closing `</body>` tag because JS 
  blocks rendering — it can only load one file at a time

### Image Sprites
An image sprite combines multiple small images into one single image file. 
CSS then uses `background-position` to show only the relevant portion of the 
sprite on each element. This turns many HTTP requests into just one.

### Image Data URIs
Instead of linking to an image file, you can encode the image directly into 
your HTML or CSS as a base64 string. This eliminates the HTTP request entirely. 
Best for small images that rarely change. Does not work in IE7 and below.

---

## Cache Common Files

Browsers can be told to store (cache) files locally so they don't need to 
re-request them on every visit. Caching is configured in `.htaccess` using 
expires headers.

Common caching windows:
- Images, video, fonts — cached for ~1 month
- CSS and JavaScript — cached for ~1 year

If a CSS file changes more frequently than its cache window, version the 
filename (e.g. `styles-v2.css`) so the browser fetches the updated file.





# Shay Howe Advanced HTML & CSS — Lesson 2: Detailed Positioning

## Containing Floats

When elements are floated inside a parent container, the parent essentially 
forgets they are there and collapses to a height of 0. This is one of the most 
common float problems. The content on the page respects the floated children, 
but the parent container loses track of them entirely. You may not notice this 
until the parent has a background color or border — then the collapse becomes 
very obvious.

There are three ways to deal with this:

**1. Empty div with clear: both (not recommended)**
Placing an empty `<div style="clear: both;"></div>` before the closing tag of 
the parent works but it's not semantic — you're adding meaningless HTML just 
to fix a CSS problem. This is the approach you used in the layout exercises, 
and it's fine for learning, but not ideal in production.

**2. The Overflow Technique**
Adding `overflow: auto` to the parent element forces it to contain its floated 
children and gives it a real height. Simple and clean, but has drawbacks — it 
can clip box shadows and dropdown menus that extend outside the parent, and 
different browsers handle it differently.

```css
.box-set {
  overflow: auto;
}
```

**3. The Clearfix Technique (preferred)**
The clearfix uses CSS pseudo-elements `:before` and `:after` on the parent to 
create hidden elements that contain the floats without adding extra HTML. 
The `:after` pseudo-element does the actual clearing. This is the most widely 
used and reliable method.

```css
.group:before,
.group:after {
  content: "";
  display: table;
}
.group:after {
  clear: both;
}
.group {
  *zoom: 1;
}
```

The convention is to name this reusable class `group` (coined by Dan Cederholm) 
and apply it to any parent that needs to contain floats. Note: each element 
only gets one `:before` and one `:after` pseudo-element, so if you're already 
using them for something else, you'll need a different approach.

---

## The Position Property

The `position` property gives you more precise control over element placement 
than floats can provide. It accepts five values.

### Static (default)
Every element is `position: static` by default. Static elements follow normal 
document flow and do not accept box offset properties (`top`, `right`, 
`bottom`, `left`). Nothing special happening here — elements stack and flow 
as expected.

### Relative
`position: relative` keeps the element in the normal document flow but allows 
you to shift it from its original position using box offset properties. 
Importantly, the space it originally occupied is preserved — surrounding 
elements don't move to fill it. The element can overlap others without pushing 
them around.

```css
.box {
  position: relative;
  top: 20px;   /* pushes DOWN 20px from original position */
  left: 40px;  /* pushes RIGHT 40px from original position */
}
```

If both `top` and `bottom` are set, `top` wins. If both `left` and `right` 
are set, the direction of the page language wins (left for English).

### Absolute
`position: absolute` removes the element completely from normal document flow. 
Other elements act as if it doesn't exist. The element positions itself 
relative to its closest parent that has `position: relative` or 
`position: absolute`. If no such parent exists, it positions relative to the 
`<body>`.

Box offset properties on absolutely positioned elements describe distance 
from the edges of the parent, not from the element's original position.

```css
.parent {
  position: relative;  /* establishes the positioning context */
}
.child {
  position: absolute;
  top: 50px;    /* 50px from top of parent */
  right: 100px; /* 100px from right of parent */
}
```

If an absolutely positioned element has no fixed height and both `top` and 
`bottom` are set, it will stretch to fill that space. Same for `left`/`right` 
and width.

### Fixed
`position: fixed` works like absolute but positions the element relative to 
the browser viewport, not a parent element. It does not scroll with the page — 
it stays in place as the user scrolls. This is how sticky headers and fixed 
footers are built.

```css
footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
}
```

Setting both `left: 0` and `right: 0` on a fixed element stretches it across 
the full width of the viewport without disrupting the box model.

---

## The Z-Index Property

Web pages exist on an x and y axis, but when elements overlap each other, 
there is also a z-axis — think of it as depth, which element sits on top. 
By default, elements later in the DOM stack on top of earlier ones.

The `z-index` property lets you control that stacking order. A higher number 
sits on top. `z-index` only works on elements that have a `position` value of 
`relative`, `absolute`, or `fixed` — it has no effect on static elements.

```css
.box-2 { z-index: 3; }  /* on top */
.box-3 { z-index: 2; }  /* middle */
.box-4 { z-index: 1; }  /* bottom */
```