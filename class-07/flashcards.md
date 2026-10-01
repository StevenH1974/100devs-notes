# Anki Flashcards — Shay Howe Advanced Lesson 1: Performance & Organization
## Tags: HTML, CSS, performance, organization

---

Q: What are the three directory categories in Shay Howe's recommended CSS architecture?
A: Base (global defaults like normalize and typography), Components (reusable UI elements like buttons and forms), and Modules (page sections driven by business logic like header and footer).

---

Q: What are the two core principles of OOCSS (Object Oriented CSS)?
A: 1. Separate structure from skin — keep layout independent from visual theme. 2. Separate content from container — an element should look the same regardless of its parent.

---

Q: What are the five categories in SMACSS?
A: Base, Layout, Module, State, and Theme.

---

Q: Why should CSS selectors be kept short?
A: Browsers read selectors from right to left. Long selectors force the browser to evaluate every level, slowing rendering. Short selectors — especially classes — are faster and more maintainable.

---

Q: Why should ID selectors be avoided in CSS where possible?
A: IDs are overly specific, cannot be reused, and behave similarly to !important — they make styles harder to override and less flexible.

---

Q: What is gzip compression and what does it do?
A: gzip is a server-side compression method that finds repeated strings in files like HTML, CSS, and JavaScript and compresses them, reducing file sizes by up to 60%. It is configured via an .htaccess file on Apache servers.

---

Q: Why should JavaScript be loaded at the bottom of the page just before the closing body tag?
A: JavaScript blocks rendering — the browser can only load one JS file at a time. Loading it at the bottom allows the rest of the page to render first before JS executes.

---

Q: What is an image sprite and why is it used?
A: An image sprite combines multiple small images into one single image file. CSS uses background-position to display the correct portion on each element. This reduces multiple HTTP requests down to one, improving load time.

---

Q: What is a Data URI in the context of images?
A: A Data URI encodes an image directly into HTML or CSS as a base64 string, eliminating the HTTP request for that image entirely. Best for small images that rarely change.

---

Q: What is browser caching and how long are CSS and JavaScript files typically cached?
A: Browser caching stores files locally so repeat visitors don't have to re-download them. CSS and JavaScript files are typically cached for one year. If files change more often, the filename should be versioned (e.g. styles-v2.css) to force a fresh download.




# Anki Flashcards — Shay Howe Advanced Lesson 2: Detailed Positioning
## Tags: HTML, CSS, positioning, floats

---

Q: What problem occurs when all children inside a parent element are floated?
A: The parent collapses to a height of 0 because floated elements are removed 
from normal flow and the parent no longer recognizes them as taking up space.

---

Q: What are the three techniques for containing floats inside a parent element?
A: 1. Empty div with clear: both (not semantic, not recommended for production). 
2. overflow: auto on the parent (simple but can clip shadows and dropdowns). 
3. The clearfix technique using :before and :after pseudo-elements (preferred).

---

Q: What is the clearfix technique and what class name is conventionally used for it?
A: The clearfix uses CSS :before and :after pseudo-elements on the parent to 
create hidden elements that contain floats without extra HTML. The conventionally 
used class name is "group" coined by Dan Cederholm.

---

Q: What is the default position value for all HTML elements, and what does it mean?
A: The default is position: static. It means the element follows normal document 
flow and does not accept box offset properties (top, right, bottom, left).

---

Q: What makes position: relative different from position: static?
A: Relative accepts box offset properties and lets you shift an element from its 
original position. However the element still occupies its original space in the 
document flow — surrounding elements are unaffected.

---

Q: What is the key difference between position: relative and position: absolute?
A: Relative keeps the element in normal document flow and shifts it from its 
original position. Absolute removes the element from normal flow entirely, and 
positions it relative to its closest positioned parent (relative or absolute).

---

Q: If an absolutely positioned element has no positioned parent, where does it 
position itself?
A: It positions itself relative to the body of the page.

---

Q: What is position: fixed and what is its most common use case?
A: Fixed positions an element relative to the browser viewport and does not 
scroll with the page. Its most common use is building fixed headers or footers 
that stay visible as the user scrolls.

---

Q: What does the z-index property control, and what requirement must be met to use it?
A: z-index controls the stacking order of overlapping elements on the z-axis — 
higher values appear on top. It only works on elements with position: relative, 
absolute, or fixed. It has no effect on static elements.

---

Q: When using box offset properties, what happens if both top and bottom are 
declared on the same element?
A: The top property takes priority on elements with a fixed height. If no height 
is set on an absolutely positioned element, the element stretches to fill the 
space between both values.