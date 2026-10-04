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




# Anki Flashcards — Shay Howe Advanced Lesson 5: Preprocessors
## Tags: CSS, Sass, SCSS, preprocessors

---

Q: What is a preprocessor in the context of HTML and CSS?
A: A program that converts one type of code into another. Haml converts to HTML
and Sass/SCSS converts to CSS, adding features like variables, nesting, and
logic that plain CSS doesn't support.

---

Q: What is the difference between Sass and SCSS?
A: Both compile to CSS and share the same features. SCSS uses standard CSS
syntax with curly braces and semicolons and accepts plain CSS. Sass uses strict
indentation with no curly braces or semicolons — cleaner but less forgiving.

---

Q: How do you define and use a variable in Sass?
A: Variables are defined with a dollar sign: $color-primary: #0087cc.
They are then used anywhere in the stylesheet by referencing the variable
name: color: $color-primary.

---

Q: What does the parent selector (&) do in Sass?
A: It references the parent selector, allowing you to attach pseudo-classes or
additional selectors without repeating the parent name. Example: a { &:hover
{ color: red; } } compiles to a:hover { color: red; }

---

Q: What is the difference between @extend and a mixin in Sass?
A: @extend makes one selector inherit styles from another with no arguments —
it groups selectors together in the CSS output. A mixin is a reusable style
template that accepts arguments, outputting styles separately for each selector
that calls it. Extends share fixed styles; mixins are flexible templates.

---

Q: What is a placeholder selector in Sass and why use it?
A: A placeholder selector starts with % and never compiles to CSS on its own.
It is used purely to be extended by other selectors, keeping the compiled CSS
output clean by not generating an unused base class.

---

Q: What does @import do in Sass and why is it useful?
A: @import pulls in other Sass partial files and compiles everything into one
single CSS file. This lets you organize your code across many files without
creating multiple HTTP requests in the browser.

---

Q: Name three HSLa color functions available in Sass.
A: lighten(color, %) makes a color lighter, darken(color, %) makes it darker,
and fade-out(color, amount) reduces its opacity. Others include saturate(),
desaturate(), and complement().

---

Q: What does the Sass @for loop do and what is the difference between "to" and "through"?
A: @for outputs styles repeatedly based on a counter variable. "to" counts up
to but not including the end number. "through" counts up to and including
the end number.

---

Q: What is Haml and what is its main advantage over writing plain HTML?
A: Haml is an HTML preprocessor that compiles to standard HTML. Its main
advantage is eliminating closing tags and enforcing clean structure through
indentation, making markup faster to write and easier to scan.




# Anki Flashcards — Shay Howe Advanced Lesson 6: jQuery
## Tags: JavaScript, jQuery, DOM

---

Q: What are the three roles of HTML, CSS, and JavaScript in a web page?
A: HTML provides structure, CSS provides appearance, and JavaScript provides
behavior and interactivity.

---

Q: Where should JavaScript and jQuery script tags be placed in HTML and why?
A: Just before the closing </body> tag. This allows all HTML to parse first
before the scripts execute, preventing errors from trying to interact with
elements that haven't loaded yet.

---

Q: What is the jQuery document ready function and why is it important?
A: $(document).ready(function() { }); — it wraps all jQuery code to ensure
it doesn't run until the DOM has fully loaded. Without it, jQuery may try
to select elements that don't exist yet.

---

Q: What is the jQuery $ object and how is it used?
A: The $ is the jQuery object, used to select elements and return them for
manipulation. You pass a CSS-style selector inside $() to target elements:
$('.feature') selects all elements with the class "feature."

---

Q: What is the this keyword in a jQuery event handler?
A: Inside a jQuery event function, $(this) refers to the specific element
that triggered the event, allowing you to act on just that element rather
than all matching elements.

---

Q: What is the difference between getting and setting in jQuery manipulation methods?
A: Passing no value argument gets the current value: $('img').attr('alt')
returns the alt text. Passing a value sets it: $('img').attr('alt', 'New
text') changes the alt text. Same method, different number of arguments.

---

Q: What is the .on() method in jQuery and why is it preferred over shorthand event methods?
A: .on() is the flexible event handler method. The first argument is the
event name and the second is the handler function. It is preferred because
it supports dynamic delegation for elements added to the page after load,
unlike shorthand methods like .click().

---

Q: What does event.preventDefault() do in a jQuery event handler?
A: It stops the browser's default behavior for that event — for example,
preventing a link from navigating to a new page or a form from submitting.

---

Q: What are the three parameters that jQuery effect methods accept?
A: Duration (a keyword like 'slow' or 'fast', or milliseconds), easing
('swing' or 'linear'), and a callback function that runs after the effect
completes. All three are optional.

---

Q: What is the difference between swing and linear easing in jQuery effects?
A: Swing (the default) starts the animation slow, speeds up in the middle,
then slows again at the end. Linear runs the animation at one constant
speed for the entire duration.



# Anki Flashcards — Shay Howe Advanced Lesson 7: CSS Transforms
## Tags: CSS, transforms, 2D, 3D

---

Q: What are the four 2D transform values and what does each do?
A: rotate() — spins an element clockwise (positive) or counterclockwise
(negative) in degrees. scale() — changes the apparent size. translate() —
moves the element without affecting document flow. skew() — distorts the
element along an axis in degrees.

---

Q: How do you combine multiple transforms on one element?
A: List them space-separated on a single transform property:
transform: rotate(25deg) scale(.75); — Never use multiple separate transform
declarations because each one overwrites the previous.

---

Q: What is the default transform-origin and how do you change it?
A: The default is 50% 50% — the center of the element. Change it with the
transform-origin property using keywords (top left), percentages (0% 100%),
or pixel values (20px 50px).

---

Q: What is perspective in CSS transforms and what are the two ways to apply it?
A: Perspective sets the depth/vanishing point required for 3D transforms to
look three-dimensional. Apply it as perspective() inside the transform property
on individual elements, or as the perspective property on a parent element so
all children share the same vanishing point.

---

Q: What is the visual difference between a low and high perspective value?
A: A low perspective value (e.g. 100px) makes the 3D effect dramatic and
intense — like viewing an object up close. A high value (e.g. 1000px) makes
the effect subtle — like viewing the same object from far away.

---

Q: What do rotateX, rotateY, and rotateZ do differently?
A: rotateX tilts the element as if bending it horizontally (top and bottom
tip toward or away from you). rotateY tilts it as if bending vertically (left
and right sides tip). rotateZ spins it flat on the screen like a 2D rotation.

---

Q: What does translateZ do and how does it differ from scale?
A: translateZ moves an element along the z axis — negative pushes it further
away (appearing smaller), positive pulls it closer (appearing larger). Unlike
scale, it is a true depth movement in 3D space, not just a size change.

---

Q: What is transform-style: preserve-3d and when do you need it?
A: It is applied to a parent element to allow its transformed children to
maintain their own 3D space. Without it, children of a transformed parent
collapse flat into a 2D plane, losing their depth.

---

Q: What does backface-visibility: hidden do?
A: It hides an element when it is rotated to face away from the screen —
for example after a rotateY(180deg). Without it the element's back side
is visible by default. Most commonly used in card-flip animations.

---

Q: Which 2D transform cannot be applied on the z axis in 3D transforms?
A: Skew. There is no skewZ — elements can be skewed on the x and y axes
only.




# Anki Flashcards — Shay Howe Advanced Lesson 8: Transitions & Animations
## Tags: CSS, transitions, animations, keyframes

---

Q: What are the four CSS transition properties?
A: transition-property (what to animate), transition-duration (how long),
transition-timing-function (speed curve), and transition-delay (wait before
starting). The first three are most commonly used.

---

Q: What triggers a CSS transition?
A: A state change on the element — most commonly the :hover, :focus, :active,
or :target pseudo-classes. Without a state change, there is nothing to
transition between.

---

Q: Why can't every CSS property be transitioned?
A: Only properties that have a calculable midpoint can be transitioned. Colors,
sizes, opacity, and position values have clear halfway points. The display
property, for example, has no midpoint between block and none, so it cannot
be transitioned.

---

Q: What are the four transition timing function keyword values and what does each do?
A: linear — constant speed throughout. ease-in — starts slow, speeds up.
ease-out — starts fast, slows down. ease-in-out — slow start, fast middle,
slow end.

---

Q: What is the shorthand transition syntax and what order do the values go in?
A: transition: property duration timing-function delay — for example:
transition: background .2s linear 1s. Comma-separate multiple transitions.

---

Q: What is the key difference between a CSS transition and a CSS animation?
A: Transitions move between two states and require a trigger like :hover.
Animations use @keyframes to define multiple states, can run automatically
without a trigger, can loop indefinitely, and offer more control over
direction and timing.

---

Q: What does the @keyframes rule do and how do you write it?
A: @keyframes defines the stages of an animation using percentage breakpoints.
@keyframes slide { 0% { left: 0; } 100% { left: 100px; } } — you then
apply it to an element with animation-name and animation-duration.

---

Q: What does animation-fill-mode: forwards do?
A: It holds the styles from the final keyframe after the animation finishes,
so the element stays in its end state rather than snapping back to its
original styles.

---

Q: What does animation-direction: alternate do?
A: It plays the animation forward (0% to 100%) then backward (100% to 0%),
bouncing back and forth. Each back-and-forth counts as two iterations.

---

Q: What does animation-play-state do and what are its two values?
A: It controls whether an animation is running or paused. The values are
"running" (plays normally) and "paused" (freezes the animation at its
current position, resuming from there when unpaused).





