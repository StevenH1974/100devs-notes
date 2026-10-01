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