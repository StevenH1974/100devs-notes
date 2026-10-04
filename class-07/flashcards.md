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




# Anki Flashcards — Shay Howe Advanced Lesson 3: Complex Selectors
## Tags: CSS, selectors, pseudo-classes, pseudo-elements

---

Q: What is the difference between a descendant selector and a direct child selector?
A: A descendant selector (space) targets an element nested anywhere inside an
ancestor at any depth. A direct child selector (>) only targets elements that
are immediate children of the parent, not deeper descendants.

---

Q: What is the difference between the general sibling selector and the adjacent sibling selector?
A: The general sibling selector (~) selects all matching siblings that appear
anywhere after the first element. The adjacent sibling selector (+) only selects
the element that comes immediately after, with nothing in between.

---

Q: What does the attribute selector a[href$=".pdf"] select?
A: It selects any anchor element whose href attribute value ends with ".pdf".
The $ symbol means "ends with."

---

Q: Write the attribute selectors for: attribute is present, contains a value, and begins with a value.
A: Present: a[target] — Contains: a[href*="login"] — Begins with: a[href^="https://"]

---

Q: What are the three user action pseudo-classes and when does each apply?
A: :hover applies when the cursor is over the element. :active applies when the
element is being clicked. :focus applies when the element is focused, such as
when tabbed to via keyboard.

---

Q: What is the difference between :first-child and :first-of-type?
A: :first-child selects an element only if it is the first child of its parent,
regardless of type. :first-of-type selects the first element of that specific
type within a parent, even if other element types come before it.

---

Q: What does the nth-child expression li:nth-child(-n+4) select?
A: It selects only the first four list items in a list, leaving all others
unselected. The negative n with a positive b value limits the selection to
a maximum count from the beginning.

---

Q: What is the :not() pseudo-class and give an example?
A: The negation pseudo-class selects elements that do NOT match the argument
inside the parentheses. Example: div:not(.awesome) selects every div that
does not have the class "awesome."

---

Q: What is the difference between pseudo-classes and pseudo-elements?
A: Pseudo-classes (single colon) target elements based on state or position in
the document tree. Pseudo-elements (double colon) target specific parts of an
element's content, like the first letter or generated content before/after it.

---

Q: What is the ::selection pseudo-element and what properties can be used with it?
A: ::selection styles text that the user has highlighted on the page. Only
color, background, background-color, and text-shadow can be applied to it.
It must always use double colons.




# Anki Flashcards — Shay Howe Advanced Lesson 4: Responsive Web Design
## Tags: CSS, responsive, media-queries, RWD

---

Q: What are the three main components of responsive web design?
A: Flexible layouts (using relative units like percentages), media queries
(applying different styles based on viewport conditions), and flexible media
(scaling images and videos with the viewport).

---

Q: What is the formula for converting a fixed pixel width to a flexible percentage?
A: target ÷ context = result. Divide the element's target width by its
parent container's width to get the percentage value to use instead.

---

Q: What is the difference between responsive and mobile web design approaches?
A: Responsive builds one fluid website that adapts to any screen size.
Mobile builds a completely separate website on a different domain specifically
for mobile users — generally not recommended due to the extra code base and
maintenance overhead.

---

Q: What is the recommended way to include media queries and why?
A: Use the @media rule inside your existing stylesheet. This avoids creating
additional HTTP requests that a separate linked stylesheet would cause.

---

Q: What is the mobile first approach to media queries?
A: Write default CSS for small screens first, then use min-width media queries
to progressively add styles for larger viewports. This avoids making mobile
users download unnecessary desktop styles that just get overwritten.

---

Q: Why should you NOT set media query breakpoints at common device widths like 320px or 768px?
A: New devices with different resolutions are released constantly, making
device-based breakpoints an endless moving target. Breakpoints should only
be added when the layout actually starts to break or look wrong.

---

Q: What does the standard viewport meta tag do and what is its recommended value?
A: It tells mobile browsers how to handle the page width so media queries
work correctly. The standard value is: content="width=device-width, initial-scale=1"

---

Q: Why is setting user-scalable=no in the viewport meta tag a bad practice?
A: It disables the user's ability to zoom, which harms accessibility and
usability — particularly for users with visual impairments who rely on zooming.

---

Q: How do you make a standard image responsive?
A: Set max-width: 100% on the image. This ensures it never exceeds its
container width and scales down proportionally as the viewport shrinks.

---

Q: What technique is used to make embedded iframes like YouTube videos responsive?
A: The aspect ratio padding trick. The parent element gets height: 0, width: 100%,
and padding-bottom set to the aspect ratio percentage (56.25% for 16:9 video).
The iframe is then absolutely positioned to fill that space completely.