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




# Shay Howe Advanced HTML & CSS — Lesson 3: Complex Selectors

Selectors are one of the most important parts of CSS. CSS3 introduced a whole
new set of selectors that give developers much more precise control over what
gets styled and when.

---

## Common Selectors (Review)

The three foundational selectors you already know:
- **Type selector** — targets elements by their HTML tag: `h1 {...}`
- **Class selector** — targets by class attribute, reusable: `.tagline {...}`
- **ID selector** — targets by ID attribute, unique per page: `#intro {...}`

---

## Child Selectors

### Descendant Selector (space)
Selects any matching element nested anywhere inside an ancestor, no matter
how deep. Written with a space between the ancestor and target.

```css
article h2 { ... }  /* selects ALL h2 inside article, at any depth */
```

### Direct Child Selector (>)
More specific — only selects elements that are immediate children of the
parent, not deeply nested ones.

```css
article > p { ... }  /* only selects p directly inside article */
```

---

## Sibling Selectors

Siblings are elements that share the same parent.

### General Sibling Selector (~)
Selects all matching siblings that appear anywhere after the first element,
as long as they share the same parent.

```css
h2 ~ p { ... }  /* selects any p that comes after an h2, same parent */
```

### Adjacent Sibling Selector (+)
More strict — only selects the element that comes immediately after another,
with no elements in between.

```css
h2 + p { ... }  /* only the p directly after an h2, same parent */
```

Note: you used `li + li::before` in the simple-site-lab to add pipe separators
between nav items — that was the adjacent sibling selector in action.

---

## Attribute Selectors

Attribute selectors let you target elements based on their HTML attributes
and attribute values, not just their type or class.

| Selector | What it does |
|---|---|
| `a[target]` | Element has the attribute present, any value |
| `a[href="url"]` | Attribute value matches exactly |
| `a[href*="login"]` | Attribute value contains the string |
| `a[href^="https://"]` | Attribute value begins with the string |
| `a[href$=".pdf"]` | Attribute value ends with the string |
| `a[rel~="tag"]` | Attribute is space-separated, one word matches exactly |
| `a[lang\|="en"]` | Attribute is hyphen-separated, begins with the word |

A practical use: automatically adding icons to links based on file type.
```css
a[href$=".pdf"] { background-image: url("pdf-icon.png"); }
a[href$=".mp3"] { background-image: url("audio-icon.png"); }
```

---

## Pseudo-classes

Pseudo-classes are not written in the HTML — they are dynamically applied
based on user actions or document structure. They always start with a colon `:`.

### Link Pseudo-classes
```css
a:link { ... }     /* unvisited link */
a:visited { ... }  /* link user has already visited */
```

### User Action Pseudo-classes
```css
a:hover { ... }   /* cursor is over the element */
a:active { ... }  /* element is being clicked */
a:focus { ... }   /* element is focused (e.g. tabbed to via keyboard) */
```

### UI State Pseudo-classes (Form Elements)
```css
input:enabled { ... }       /* input available for use */
input:disabled { ... }      /* input with disabled attribute */
input:checked { ... }       /* checked checkbox or radio button */
input:indeterminate { ... } /* neither checked nor unchecked */
```

### Structural & Position Pseudo-classes
These select elements based on where they sit in the document tree.

```css
li:first-child { ... }     /* first child of its parent */
li:last-child { ... }      /* last child of its parent */
div:only-child { ... }     /* only child of its parent */

p:first-of-type { ... }    /* first p within its parent */
p:last-of-type { ... }     /* last p within its parent */
img:only-of-type { ... }   /* only img within its parent */
```

### nth Pseudo-classes
These accept a number or algebraic expression to select elements in patterns.

```css
li:nth-child(3)      /* selects the 3rd list item */
li:nth-child(odd)    /* selects all odd items */
li:nth-child(even)   /* selects all even items */
li:nth-child(3n)     /* selects every 3rd item */
li:nth-child(2n+3)   /* every 2nd item starting from the 3rd */
li:nth-child(-n+4)   /* only the first 4 items */
```

The expression format is `an+b` where `a` is the interval and `b` is the
starting point. `:nth-last-child` counts from the end of the list instead.

`:nth-of-type(n)` works the same but only counts elements of the same type,
skipping any sibling elements of different types.

### Other Useful Pseudo-classes
```css
section:target { ... }       /* element whose ID matches the URL hash */
div:empty { ... }            /* element with no children or text */
div:not(.awesome) { ... }    /* any div WITHOUT the class "awesome" */
```

---

## Pseudo-elements

Pseudo-elements are similar to pseudo-classes but they target specific parts
of an element rather than the element itself. In CSS3 they use double colons
`::` to distinguish them from pseudo-classes, though single colon still works
in most browsers (except `::selection`).

Only one pseudo-element is allowed per selector at a time.

### Textual Pseudo-elements
```css
p:first-letter { ... }  /* styles just the first letter */
p:first-line { ... }    /* styles just the first line of text */
```

### Generated Content Pseudo-elements
These create virtual elements before or after the selected element's content.
Used constantly — you used `:after` in the clearfix technique.

```css
a:before { content: "→ "; }          /* inserts before the link text */
a:after { content: " (" attr(href) ")"; }  /* appends the URL after */
```

The `content` property is required and can hold text, the value of an
attribute using `attr()`, or be left as `""` for layout purposes like clearfix.

### Fragment Pseudo-element
```css
::selection { background: orange; }  /* styles highlighted/selected text */
```

Only `color`, `background`, `background-color`, and `text-shadow` work here.
Must use double colons.




# Shay Howe Advanced HTML & CSS — Lesson 4: Responsive Web Design

Responsive web design (RWD) is the practice of building websites that work
on every device and screen size. The term was coined by Ethan Marcotte.
RWD is built on three pillars: flexible layouts, media queries, and flexible media.

**Responsive vs. Adaptive vs. Mobile:**
- Responsive — fluidly changes based on viewport width continuously
- Adaptive — built to a group of preset sizes/factors
- Mobile — a completely separate website on a different domain for mobile users
  (generally not a great approach)

The industry standard is a combination of responsive and adaptive techniques.

---

## Flexible Layouts

A flexible layout uses relative units (percentages or em) instead of fixed
pixels so the layout scales proportionally with the viewport.

**The formula for converting fixed to flexible:**

Divide the target element's width by its parent container's width to get
the percentage to use.

Example: a section that is 340px inside a 538px container:
340 ÷ 538 = 63.197% — use that as the width instead of 340px.

**Viewport-relative units (CSS3):**
- `vw` — 1% of the viewport width
- `vh` — 1% of the viewport height
- `vmin` — 1% of the smaller of width or height
- `vmax` — 1% of the larger of width or height

Flexible layouts alone aren't always enough. When a viewport gets very small,
even proportionally scaled columns can become too narrow to be usable. That's
where media queries come in.

---

## Media Queries

Media queries let you apply different CSS styles based on the characteristics
of the browser or device — most commonly viewport width. They are the backbone
of responsive web design.

**Best practice:** write media queries using the `@media` rule inside your
existing stylesheet to avoid extra HTTP requests.

```css
@media all and (max-width: 1024px) {
  /* styles for viewports 1024px wide and smaller */
}
```

### Logical Operators
- `and` — both conditions must be true
- `not` — negates the query
- `only` — hides styles from browsers that don't support media queries
- Comma-separated queries act as an **or** operator

```css
/* Between 800px and 1024px wide */
@media all and (min-width: 800px) and (max-width: 1024px) { ... }

/* Portrait orientation only */
@media only screen and (orientation: portrait) { ... }
```

### Common Media Features
- `min-width` / `max-width` — most commonly used for responsive layouts
- `min-height` / `max-height`
- `orientation: portrait` or `orientation: landscape`
- `min-resolution` — targets devices by DPI (useful for print)
- `device-pixel-ratio` — targets retina/high-DPI screens

### Breakpoints
**Do NOT** set breakpoints at specific device sizes like 320px, 768px, 1024px.
Devices change constantly. Instead, add a breakpoint **only when the layout
starts to break or look wrong.** Let the content decide when a breakpoint
is needed.

---

## Mobile First

Mobile first is a design strategy where you write your default CSS for small
screens, then use media queries with `min-width` to progressively add styles
for larger viewports.

**Why mobile first:**
- Mobile users don't have to download desktop styles only to have them
  overwritten — saves bandwidth
- Forces you to design with mobile constraints in mind from the start
- The majority of internet usage is now on mobile devices

```css
/* Default mobile styles */
section { width: 100%; }

/* Add styles as viewport grows */
@media screen and (min-width: 420px) {
  section { float: left; width: 63%; }
  aside  { float: right; width: 29%; }
}
```

This is the opposite of the traditional desktop-first approach where you
start wide and use `max-width` to scale down.

---

## Viewport Meta Tag

Even with media queries in place, mobile browsers need to be told how to
handle the page width. Apple invented the viewport meta tag for this.
Without it, a mobile browser may zoom out to show the full desktop-width
page, completely ignoring your media queries.

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

This is the standard recommended setting — it's the boilerplate you see
in every HTML file. `width=device-width` tells the browser to match the
viewport to the device width. `initial-scale=1` sets the default zoom to 1.

**Other viewport properties:**
- `minimum-scale` / `maximum-scale` — limits how far users can zoom
- `user-scalable=no` — disables zooming entirely (bad practice — hurts
  accessibility)
- `target-densitydpi` — rare, controls pixel density

---

## Flexible Media

Images, videos, and other media also need to scale with the viewport.
The simplest fix is:

```css
img, video, canvas {
  max-width: 100%;
}
```

This ensures media never exceeds its container width and scales down naturally.

### Flexible Embedded Media (iframes)
`max-width: 100%` doesn't work on iframes. For embedded YouTube videos and
similar third-party iframes, use the aspect ratio padding trick:

```css
figure {
  height: 0;
  padding-bottom: 56.25%; /* 16:9 ratio: 9 ÷ 16 = 0.5625 */
  position: relative;
  width: 100%;
}
iframe {
  height: 100%;
  left: 0;
  position: absolute;
  top: 0;
  width: 100%;
}
```

The parent has `height: 0` and bottom padding set to the aspect ratio
percentage. The iframe is then absolutely positioned to fill that space.
This keeps the video perfectly proportioned at any viewport size.





# Shay Howe Advanced HTML & CSS — Lesson 5: Preprocessors

A preprocessor takes one type of data and converts it to another. In web
development, Haml converts to HTML and Sass/SCSS converts to CSS. They exist
to remove repetition, add logic, and make code more maintainable.

---

## Haml (HTML Abstraction Markup Language)

Haml is an alternative way to write HTML that compiles down to standard HTML.
It promotes cleaner, more readable markup by eliminating closing tags and
enforcing structure through indentation.

**Key Haml syntax rules:**
- Elements start with `%` — `%h1`, `%section`, `%p`
- Nesting is done through indentation — no closing tags needed
- Classes use `.` directly after the element — `%section.feature`
- IDs use `#` directly after the element — `%section#hello`
- For divs specifically, you can omit `%div` and just use `.classname` or `#id`
- Attributes go in `{}` (Ruby style) or `()` (HTML style)
- Comments use `/` — block comments nest underneath it
- Silent comments use `-#` and are completely removed from compiled output

```haml
%body
  %header
    %h1 Hello World
  %section
    %p Lorem ipsum dolor sit amet.
```

Compiles to standard HTML with opening and closing tags. Haml requires Ruby
to compile and saves as `.haml` files.

---

## Sass & SCSS

Sass (Syntactically Awesome Stylesheets) and SCSS (Sassy CSS) are CSS
preprocessors that compile to standard CSS. SCSS is the more flexible of the
two — it accepts plain CSS syntax. Sass is stricter, uses indentation instead
of curly braces and semicolons, and is generally considered cleaner once learned.

Both use `.sass` or `.scss` file extensions and require Ruby to compile.
You can watch a file for changes with: `sass --watch styles.sass:styles.css`

### Nesting
Selectors can be nested inside each other, which compiles to descendant selectors.
Don't go overboard — only nest when it makes logical sense.

```sass
.portfolio
  border: 1px solid #9799a7
  ul
    list-style: none
  li
    float: left
```
Compiles to `.portfolio { }`, `.portfolio ul { }`, `.portfolio li { }`

### Parent Selector (&)
The `&` references the parent selector, most commonly used with pseudo-classes.

```sass
a
  color: #0087cc
  &:hover
    color: #ff7b29
```
Compiles to `a { }` and `a:hover { }`

### Variables
Variables store reusable values — colors, fonts, sizes. Defined with `$`.

```sass
$font-base: 1em
$serif: "Helvetica Neue", Arial, sans-serif

p
  font: $font-base $serif
```

### Calculations
Sass can do math directly in stylesheets — addition, subtraction,
multiplication, division. Also includes built-in functions:
- `percentage()` — converts to percentage
- `round()` — rounds to nearest whole number
- `ceil()` — rounds up
- `floor()` — rounds down
- `abs()` — absolute value

### Color Functions
Sass has powerful color tools:
- `rgba(#hexcolor, .5)` — converts hex to rgba with opacity
- `lighten(color, %)` — makes a color lighter
- `darken(color, %)` — makes a color darker
- `saturate()` / `desaturate()` — adjusts color saturation
- `fade-in()` / `fade-out()` — adjusts opacity
- `mix(color1, color2)` — blends two colors
- `complement()` — returns the complementary color
- `grayscale()` — converts to grayscale

### Extends (@extend)
Extends let one selector inherit styles from another without duplicating code.

```sass
.alert
  border-radius: 10px
  padding: 10px 20px

.alert-error
  @extend .alert
  background: #f2dede
```

A **placeholder selector** using `%` works the same way but never compiles
to CSS on its own — it only exists to be extended. Keeps output clean.

### Mixins (@mixin)
Mixins are like reusable style templates that can accept arguments — think
of them like functions for CSS. Called with `+mixin-name` in Sass or
`@include mixin-name` in SCSS.

```sass
@mixin btn($color, $color-hover)
  color: $color
  &:hover
    color: $color-hover

.btn
  +btn(#fff, #9799a7)
```

Mixins can have default argument values and even accept variable numbers
of arguments using `...`.

### Imports (@import)
Sass can import multiple partial files and compile them into one single CSS
file — reducing HTTP requests while keeping your code organized across
multiple files.

```sass
@import "normalize"
@import "grid", "typography"
```

Only one CSS file needs to be linked in HTML, even though the source is
organized across many Sass files.

### Loops & Conditionals
Sass supports programming-style logic for building complex style systems:
- `@if` / `@else if` / `@else` — conditional styles
- `@for $i from 1 to 6` — loop a set number of times
- `@each $item in list` — loop through a list of values
- `@while condition` — loop until condition is false

These are most useful when building mixins, grid systems, or generating
repeated patterns of classes.

---

## Key Difference: Extends vs. Mixins
- **Extends** share a fixed set of styles between selectors — no arguments,
  groups selectors together in output
- **Mixins** are templates that accept arguments and output styles per selector —
  more flexible, more output





  # Shay Howe Advanced HTML & CSS — Lesson 6: jQuery

## JavaScript Basics

HTML gives a page structure, CSS gives it appearance, and JavaScript gives it
**behavior**. JavaScript is referenced in HTML using a `<script>` tag, ideally
placed just before the closing `</body>` tag so the HTML loads first.

```html
<script src="script.js"></script>
```

**Key JavaScript concepts:**

**Variables** — store values, defined with `var`, named in camelCase.
Cannot start with a number or use hyphens.
```js
var theStarterLeague = 125;
var foodTruck = 'Coffee';
var isActive = true;
```

**Arrays** — ordered lists stored in square brackets `[]`. Items start at
index `0`, so the third item is `[2]`.
```js
var vinyl = ['Miles Davis', 'Frank Sinatra', 'Ray Charles'];
```

**Objects** — collections of key/value pairs wrapped in curly braces `{}`.
```js
var school = {
  name: 'The Starter League',
  location: 'Merchandise Mart',
  students: 120
};
```

**Functions** — reusable blocks of code that can accept arguments.
```js
function sayHello(name) {
  return('Hello ' + name);
}
```

---

## jQuery

jQuery is an open source JavaScript library that makes selecting and
manipulating HTML elements much easier. Its syntax mimics CSS selectors,
making it approachable for anyone already familiar with CSS. Used on over
63% of the top 10,000 websites.

### Getting Started

Load jQuery from a CDN before your own script file, both just before `</body>`:
```html
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
<script src="script.js"></script>
```

The jQuery object is `$()` — everything in jQuery flows through it.

### Document Ready

Always wrap jQuery code in the document ready function to ensure the DOM
is fully loaded before jQuery tries to act on it:
```js
$(document).ready(function(event) {
  // all jQuery goes here
});
```

---

## Selectors

jQuery selects elements using the same syntax as CSS — type, class, ID,
attribute, pseudo-class selectors all work:
```js
$('.feature');             // class selector
$('li strong');            // descendant selector
$('a[target="_blank"]');   // attribute selector
$('p:nth-child(2)');       // pseudo-class selector
```

The `this` keyword inside a jQuery function refers to the element that
triggered the current event:
```js
$('div').click(function(event) {
  $(this);  // refers to the clicked div
});
```

---

## Traversing

Traversing lets you navigate the DOM from a starting selection — moving
up, down, or sideways through the tree.

```js
$('div').not('.type, .collection');         // filter out certain divs
$('div').not('.type, .collection').parent(); // chain methods together
```

Common traversal methods: `.find()`, `.children()`, `.parent()`,
`.parents()`, `.siblings()`, `.next()`, `.prev()`, `.first()`, `.last()`,
`.filter()`, `.not()`

Methods can be **chained** by connecting them with dots.

---

## Manipulation

Once elements are selected, jQuery can read or change their attributes,
styles, content, and position in the DOM.

### Getting vs. Setting
The same method can get or set depending on how many arguments are passed:
```js
$('img').attr('alt');              // GET — returns the alt value
$('img').attr('alt', 'Kangaroo'); // SET — changes the alt value
```

### Attribute Manipulation
```js
$('li:even').addClass('even-item');   // add a class
$('p').removeClass();                  // remove all classes
$('abbr').attr('title', 'Hello');      // set an attribute value
```

### Style Manipulation
```js
$('h1 span').css('font-size', 'normal');  // single property
$('div').css({ fontSize: '13px', background: '#f60' }); // multiple
$('header').height(200);                   // set height in px
```

Note: CSS property names in jQuery use camelCase — `font-size` becomes
`fontSize`.

### DOM Manipulation
```js
$('section').prepend('<h3>Featured</h3>');    // insert before content
$('a[target="_blank"]').after('<em>New window.</em>'); // insert after element
$('h1').text('Hello World');                   // replace text content
```

---

## Events

Events are actions that fire only when something specific happens — a click,
a hover, a keypress. jQuery makes binding events to elements simple.

```js
$('li').on('click', function(event) {
  $(this).addClass('saved-item');
});
```

The `.on()` method is the preferred way to attach events (more flexible
than shorthand methods like `.click()`). The first argument is the event
name, the second is the function to run.

**Common event types:** `click`, `hover`, `submit`, `keydown`, `keyup`,
`focus`, `blur`, `change`, `scroll`, `resize`

**Preventing default behavior** — stops a link from navigating or a form
from submitting:
```js
event.preventDefault();
```

---

## Effects

jQuery has built-in animation effects for showing, hiding, fading, and
sliding elements.

### Basic Effects
```js
$('.error').show();       // show element
$('.error').hide();       // hide element
$('.error').toggle();     // toggle between show/hide
```

### Fading Effects
```js
$('.error').fadeIn('slow');
$('.error').fadeOut(500);
$('.error').fadeToggle();
```

### Sliding Effects
```js
$('.panel').slideDown('slow');
$('.panel').slideUp();
$('.panel').slideToggle();
```

### Effect Parameters
Effects accept up to three optional parameters:
1. **Duration** — keyword (`'slow'` = 600ms, `'fast'` = 200ms) or milliseconds
2. **Easing** — `'swing'` (default, starts/ends slow) or `'linear'` (constant)
3. **Callback** — a function that runs after the animation completes

```js
$('.error').fadeOut('slow', 'linear', function(event) {
  $(this).remove();  // runs after the fade completes
});
```

The callback pattern is important — it ensures the next action only happens
after the animation is fully done.