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




# Shay Howe Advanced HTML & CSS — Lesson 7: CSS Transforms

The `transform` property lets you visually manipulate elements by rotating,
scaling, moving, or skewing them. Transforms come in two flavors: 2D (x and y
axes) and 3D (x, y, and z axes). Vendor prefixes should be used in production
for the best browser support, with the un-prefixed version listed last.

```css
div {
  -webkit-transform: scale(1.5);
  -moz-transform: scale(1.5);
  -o-transform: scale(1.5);
  transform: scale(1.5);
}
```

---

## 2D Transforms

### Rotate
Rotates an element clockwise (positive) or counterclockwise (negative).
Default rotation point is the center of the element.

```css
transform: rotate(20deg);   /* clockwise */
transform: rotate(-55deg);  /* counterclockwise */
```

### Scale
Changes the apparent size of an element. Default value is `1`. Values below
`1` shrink the element, values above `1` grow it.

```css
transform: scale(.75);        /* shrink to 75% */
transform: scale(1.25);       /* grow to 125% */
transform: scaleX(.5);        /* scale only width */
transform: scaleY(1.15);      /* scale only height */
transform: scale(.5, 1.15);   /* different x and y values */
```

### Translate
Moves an element from its default position without affecting document flow
— similar in concept to relative positioning. Positive values push right
and down, negative values pull left and up.

```css
transform: translateX(-10px);     /* move left */
transform: translateY(25%);       /* move down */
transform: translate(-10px, 25%); /* both axes */
```

### Skew
Distorts an element along an axis. Uses degrees, not pixels.

```css
transform: skewX(5deg);         /* distort on horizontal axis */
transform: skewY(-20deg);       /* distort on vertical axis */
transform: skew(5deg, -20deg);  /* both axes */
```

---

## Combining Transforms

Multiple transforms can be listed on one `transform` property, space-separated.
Never use multiple `transform` declarations — each one overwrites the last.

```css
/* Correct */
transform: rotate(25deg) scale(.75);

/* Wrong — only the second transform applies */
transform: rotate(25deg);
transform: scale(.75);
```

---

## Transform Origin

By default every transform happens from the center of the element (50% 50%).
`transform-origin` changes that point. Uses the same syntax as background
position — keywords, percentages, or pixel values.

```css
transform-origin: 0 0;          /* top left */
transform-origin: 100% 100%;    /* bottom right */
transform-origin: top left;     /* keyword */
transform-origin: 20px 50px;    /* specific pixel values */
```

Use `transform-origin` carefully alongside `translate` — both affect
positioning and can conflict.

---

## Perspective

Perspective is required for 3D transforms to have any visual depth. Think of
it as a vanishing point — the distance between the viewer and the element.

**Two ways to set perspective:**
1. As part of the `transform` property on an individual element:
```css
transform: perspective(200px) rotateX(45deg);
```

2. As a `perspective` property on the parent element (all children share
the same vanishing point):
```css
.parent { perspective: 200px; }
.child  { transform: rotateX(45deg); }
```

**Perspective depth:** A lower value (e.g. 100px) creates a dramatic, close-up
3D effect. A higher value (e.g. 1000px) creates a subtle, distant 3D effect.

`perspective-origin` sets the position of the vanishing point, using the
same values as `transform-origin`.

---

## 3D Transforms

### 3D Rotate
Rotates on all three axes:
```css
transform: perspective(200px) rotateX(45deg); /* tilts top/bottom */
transform: perspective(200px) rotateY(45deg); /* tilts left/right */
transform: perspective(200px) rotateZ(45deg); /* spins on flat plane */
```

### 3D Translate
Moves an element on the z axis — negative pushes further away (smaller),
positive pulls closer (larger):
```css
transform: perspective(200px) translateZ(-50px); /* push back */
transform: perspective(200px) translateZ(50px);  /* pull forward */
```

### 3D Scale
Scales on the z axis using `scaleZ`. Only visually meaningful when combined
with another 3D transform like `rotateX`.

### Note on Skew
Skew is the only 2D transform that cannot be applied on the z axis. There is
no `skewZ`.

---

## Transform Style

When a transformed parent contains transformed children, the children default
to rendering flat (losing their 3D depth). To fix this, add
`transform-style: preserve-3d` to the parent.

```css
.parent {
  transform: perspective(200px) rotateY(45deg);
  transform-style: preserve-3d;  /* lets children keep their 3D space */
}
```

---

## Backface Visibility

When an element is rotated so its back faces the screen (e.g. rotateY(180deg)),
it shows by default. Set `backface-visibility: hidden` to hide it when facing
away — essential for card-flip animations.

```css
.card-back {
  backface-visibility: hidden;
  transform: rotateY(180deg);
}
```




# Shay Howe Advanced HTML & CSS — Lesson 8: Transitions & Animations

CSS3 gave us the ability to build interactions and animations entirely in CSS
without needing JavaScript or Flash. The key difference between the two:
transitions handle a change between two states, while animations can define
multiple states across multiple keyframes.

---

## Transitions

A transition fires when an element changes state — most commonly triggered
by `:hover`, `:focus`, `:active`, or `:target` pseudo-classes. There are
four transition properties:

### transition-property
Specifies which CSS property (or properties) will be animated. Use `all`
to transition everything, or comma-separate specific properties.

```css
transition-property: background, border-radius;
```

**Important:** Not every CSS property can be transitioned — only properties
that have a calculable midpoint. Colors, sizes, opacity, and positions work.
The `display` property does not.

### transition-duration
How long the transition takes. Set in seconds (`s`) or milliseconds (`ms`).
Multiple durations can be set for multiple properties, comma-separated, and
they match up in order with the `transition-property` list.

```css
transition-duration: .2s, 1s;  /* background gets .2s, border-radius gets 1s */
```

### transition-timing-function
Controls the speed curve of the transition:
- `linear` — constant speed start to finish
- `ease-in` — starts slow, speeds up
- `ease-out` — starts fast, slows down
- `ease-in-out` — slow start, fast middle, slow end

```css
transition-timing-function: ease-in-out;
```

### transition-delay
Waits a set amount of time before starting the transition.

```css
transition-delay: 1s;
```

### Shorthand Transition
The order is: property, duration, timing-function, delay. Comma-separate
multiple transitions.

```css
transition: background .2s linear, border-radius 1s ease-in 1s;
```

---

## Animations

Animations are more powerful than transitions — they can have multiple
keyframe states and run on their own without requiring a state change trigger.

### @keyframes
Define the animation's stages using percentages (or `from`/`to` for 0%/100%).

```css
@keyframes slide {
  0%   { left: 0;     top: 0; }
  50%  { left: 244px; top: 100px; }
  100% { left: 488px; top: 0; }
}
```

The `@keyframes` rule needs vendor prefixes in production
(`@-webkit-keyframes`, `@-moz-keyframes`, `@-o-keyframes`).

### Applying an Animation
Attach the animation to an element using `animation-name`, then set a
`animation-duration` — both are required for the animation to run.

```css
.ball {
  animation-name: slide;
  animation-duration: 2s;
  animation-timing-function: ease-in-out;
  animation-delay: .5s;
}
```

### Animation Iteration Count
How many times the animation runs. Use an integer or `infinite`.

```css
animation-iteration-count: infinite;
```

### Animation Direction
Controls which direction the animation plays:
- `normal` — plays forward (0% → 100%)
- `reverse` — plays backward (100% → 0%)
- `alternate` — plays forward then backward, back and forth
- `alternate-reverse` — plays backward then forward

```css
animation-direction: alternate;
```

### Animation Play State
Pauses or resumes an animation. Useful for pausing on click.

```css
animation-play-state: paused;  /* or running */
```

### Animation Fill Mode
Controls styles before and after the animation runs:
- `none` — no styles applied outside the animation (default)
- `forwards` — holds the final keyframe styles after animation ends
- `backwards` — applies the first keyframe styles immediately, even during delay
- `both` — applies both forwards and backwards behaviors

```css
animation-fill-mode: forwards;
```

### Shorthand Animation
Order: name, duration, timing-function, delay, iteration-count, direction,
fill-mode, play-state.

```css
animation: slide 2s ease-in-out .5s infinite alternate;
```

---

## Transitions vs. Animations — Key Difference

Transitions require a trigger (a state change like `:hover`) and move between
exactly two states. Animations use `@keyframes` to define multiple states,
can run automatically without a trigger, can loop, and offer far more control
over timing and direction.




# Shay Howe Advanced HTML & CSS — Lesson 9: Feature Support & Polyfills

An important mindset shift this lesson opens with: websites do not need to
look or perform identically in every browser. Decide what level of support
is acceptable based on your actual traffic data, then work from there.

---

## What Are Shivs and Polyfills?

Both are small JavaScript plugins that add support for features not natively
supported by a specific browser. They bridge the gap between what a browser
can do and what a developer wants to use.

- **Shiv/Shim** — the terms are interchangeable, no real difference
- **Polyfill** — fills in missing browser functionality so modern code works
  in older environments

---

## HTML5 Shiv

The HTML5 Shiv (created by Remy Sharp) allows HTML5 semantic elements like
`<header>`, `<section>`, `<article>`, `<nav>`, etc. to be recognized and
styled in Internet Explorer 8 and below. Without it, IE8 treats unknown
HTML5 elements as inline elements and ignores CSS applied to them.

It should be loaded inside a **conditional comment** so it only loads for
the browsers that need it:

```html
<!--[if lt IE 9]>
  <script src="html5shiv.js"></script>
<![endif]-->
```

After loading the shiv, block-level HTML5 elements need to be explicitly
declared as `display: block` in CSS:

```css
article, aside, details, figcaption, figure,
footer, header, hgroup, nav, section, summary {
  display: block;
}
```

---

## Modernizr — Feature Detection

Modernizr is a JavaScript library that detects whether a browser supports
specific HTML5 and CSS3 features. It adds classes to the `<html>` element
indicating what is and isn't supported, which you can then target in CSS.

For example if a browser supports CSS gradients, Modernizr adds:
`class="cssgradients"` to `<html>`

If it doesn't support them, Modernizr adds:
`class="no-cssgradients"` to `<html>`

You can then write CSS for both scenarios:

```css
/* Modern browsers with gradient support */
.cssgradients button {
  background: linear-gradient(#00a2f5, #0087cc);
}

/* Older browsers without gradient support */
.no-cssgradients button {
  background: url("button.png") 0 0 no-repeat;
}
```

This approach means no styles are being overwritten and no unnecessary HTTP
requests are made.

Load Modernizr in the `<head>` of your document, after your stylesheets:
```html
<script src="modernizr.js"></script>
```

Modernizr can also include the HTML5 Shiv, eliminating the need for a
separate shiv reference.

---

## Conditionally Loading Files with Modernizr + jQuery

Modernizr can be used with jQuery to conditionally load JavaScript files
based on feature support, saving bandwidth by only loading what's needed:

```js
$(document).ready(function() {
  if (Modernizr.localstorage) {
    jQuery.getScript('storage.js');        // feature supported
  } else {
    jQuery.getScript('storage-polyfill.js'); // load the polyfill
  }
});
```

You can also conditionally load files based on media queries:

```js
if (Modernizr.mq('screen and (min-width: 640px)')) {
  jQuery.getScript('tabs.js');  // only load on wider screens
}
```

**Important:** The media query condition is only checked once when the page
loads — it does not re-check if the user resizes the browser.

---

## Cross Browser Testing

The modern browsers (Chrome, Firefox, Safari) generally perform well.
Internet Explorer is where most cross-browser issues live. Tools for
testing include virtual machines running different IE versions, which
Microsoft provides for free specifically for testing purposes.

IE8 and above have built-in developer tools. IE7 and below do not —
debugging requires tools like Firebug Lite.

The general principle: test in the browsers your actual users are using,
and make support decisions based on real traffic data rather than trying
to support everything equally.




# Shay Howe Advanced HTML & CSS — Lesson 10: Extending Semantics & Accessibility

Semantics and accessibility are built into HTML by design, but they only
deliver value when used intentionally. The core principle: use the right
element for the right job, and be an advocate for that practice with your
team and in your code.

Semantics benefit everyone — they provide shared meaning, improve
accessibility for assistive technologies, help search engines understand
content, and support interoperability across platforms and devices.

---

## Hiding Content Semantically

Using `display: none` hides content visually but is not semantically correct
— screen readers may still pick it up or behave inconsistently. The better
approach is the HTML `hidden` attribute, which semantically communicates
that content should be ignored for the time being.

```html
<!-- Correct -->
<div hidden>...</div>

<!-- Not ideal -->
<div style="display: none;">...</div>
```

---

## Text Level Semantics

### Bold Text
- `<strong>` — text of **strong importance** (warnings, critical info)
- `<b>` — **stylistically offset** text with no added importance (like
  ingredient names in a recipe)

### Italic Text
- `<em>` — **stressed emphasis** that changes the meaning of a sentence
- `<i>` — **alternate voice or tone**, like a technical term or dialog

### Underline Text
- `<ins>` — text **added to the document**, supports `cite` and `datetime`
  attributes
- `<u>` — **unarticulated annotation**, like a proper name in another language.
  Use with caution — underlines are associated with links

### Strikethrough Text
- `<del>` — text **deleted from the document**, supports `cite` and `datetime`
- `<s>` — text that is **no longer accurate or relevant** (e.g. old price)

### Other Useful Text Elements
- `<mark>` — **highlights text** for reference purposes (e.g. search results)
- `<abbr title="...">` — marks up **abbreviations** with their full value in
  the title attribute
- `<sub>` / `<sup>` — subscript and superscript for typographical conventions
- `<small>` — **side comments or small print** like copyright notices
- `<code>` — inline **code fragments**, displays in monospace font
- `<pre><code>` — **larger code blocks** preserving whitespace and formatting
- `<br>` — line break within content (addresses, poems) — not for grouping
- `<wbr>` — word break opportunity within a long word

---

## Microdata

Microdata extends HTML with structured name-value pairs that machines,
browsers, and search engines can read to provide richer information. Google
uses microdata in search results to display business addresses, hours,
ratings, and more.

Microdata uses three main attributes:

- `itemscope` — Boolean attribute that declares the scope of a microdata item,
  placed on the parent element
- `itemtype` — identifies which microdata vocabulary to use (from schema.org)
- `itemprop` — marks individual properties within the item scope

```html
<section itemscope itemtype="http://schema.org/Person">
  <h1 itemprop="name">Shay Howe</h1>
  <div itemprop="jobTitle">Designer and Front-end Developer</div>
  <a href="http://shayhowe.com" itemprop="url">shayhowe.com</a>
</section>
```

Common microdata types from schema.org include Person, Organization, Event,
and PostalAddress. Items can be nested — a Person can contain a PostalAddress
inside it with its own `itemscope` and `itemtype`.

---

## WAI-ARIA

WAI-ARIA (Web Accessibility Initiative — Accessible Rich Internet Applications)
is a W3C specification that adds roles, states, and properties to HTML elements
to make them more understandable to assistive technologies like screen readers.

### Roles
Applied using the `role` attribute, roles tell assistive technologies what
a block of content does on the page.

**Landmark roles** define major regions of a page:
- `banner` — used on `<header>` (once per page)
- `navigation` — used on `<nav>`
- `main` — used on `<main>`
- `complementary` — used on `<aside>`
- `contentinfo` — used on `<footer>` (once per page)
- `search` — used on a search form
- `form` — used on a `<form>`

```html
<header role="banner">
  <nav role="navigation">...</nav>
</header>
<article role="article">
  <section role="region">...</section>
</article>
<aside role="complementary">...</aside>
<footer role="contentinfo">...</footer>
```

Note: `<header>` and `<footer>` have no implied ARIA role — the `banner` and
`contentinfo` roles should only be applied to the top-level header and footer
directly tied to the document, not to headers/footers nested inside articles
or sections.

### States & Properties
In addition to roles, WAI-ARIA includes states and properties that describe
how content is configured — for example, whether a menu is expanded or
collapsed, or whether a form field is required. These are especially important
for dynamic content and interactive widgets.

---

## Hyperlink Attributes Worth Knowing

- `download` — prompts the browser to download the file rather than open it.
  Can be used as a Boolean or given a value to set the downloaded filename.
```html
  <a href="logo.png" download="Company-Logo">Download Logo</a>
```

- `rel` — describes the relationship between the current document and the
  linked document. Common values include `nofollow`, `author`, `license`,
  `next`, `prev`, `bookmark`, and `noopener`.
```html
  <a href="legal.html" rel="license">Terms of Use</a>
```