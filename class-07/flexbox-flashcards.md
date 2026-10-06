# Anki Flashcards — CSS Flexbox
## Source: CSS-Tricks Complete Guide to Flexbox
## Tags: CSS, flexbox, layout

---

Q: What does display: flex do and what elements does it affect?
A: It activates flexbox on the container element, making all of its direct
children flex items. It does not affect deeper nested elements — only
immediate children become flex items.

---

Q: What is the difference between the main axis and the cross axis in Flexbox?
A: The main axis is the primary direction items flow along, set by
flex-direction (horizontal by default). The cross axis is perpendicular
to the main axis. justify-content controls the main axis and align-items
controls the cross axis.

---

Q: What are the four values of flex-direction and what does each do?
A: row (default) — left to right. row-reverse — right to left.
column — top to bottom. column-reverse — bottom to top. The value you
choose becomes the main axis direction.

---

Q: What does flex-wrap do and what is its default behavior?
A: flex-wrap controls whether flex items are forced onto one line or can
wrap onto multiple lines. The default is nowrap — all items stay on one
line even if they overflow. Setting wrap allows items to flow onto
additional lines.

---

Q: What is the difference between justify-content and align-items?
A: justify-content distributes space and aligns items along the main axis.
align-items aligns items along the cross axis on a single line. Think of
justify-content as horizontal alignment and align-items as vertical
alignment when flex-direction is row.

---

Q: What is the difference between space-between, space-around, and space-evenly in justify-content?
A: space-between places the first item at the start and last at the end
with equal gaps between items — no space on outer edges.
space-around gives each item equal space on both sides, so outer edges
get half the space of inner gaps. space-evenly gives equal space between
all items AND between items and the container edges.

---

Q: What does align-content do and when does it have no effect?
A: align-content controls alignment of multiple wrapped lines along the
cross axis — similar to justify-content but for rows of wrapped items.
It has no effect on single-line containers where flex-wrap is set to
nowrap (the default).

---

Q: What does the gap property do in a flex container?
A: gap controls the space between flex items only — it does not add space
on the outer edges of the container. It is cleaner than using margins on
items because it avoids extra space on the first and last items.

---

Q: What does flex-grow do and what is its default value?
A: flex-grow defines how much of the available remaining space an item
can take up relative to other items. The default is 0 — items do not
grow. If all items have flex-grow: 1 they share space equally. An item
with flex-grow: 2 takes twice as much space as items with flex-grow: 1.

---

Q: What does flex-basis set and how does it differ from width?
A: flex-basis sets the default size of a flex item before remaining space
is distributed — it is the item's starting or "ideal" size. Unlike width,
it respects the flex layout context and works in combination with
flex-grow and flex-shrink to calculate final sizes.

---

Q: What is the flex shorthand and why is it recommended over the individual properties?
A: flex is shorthand for flex-grow, flex-shrink, and flex-basis combined.
It is recommended because the shorthand resets the other values
intelligently — for example flex: 1 sets flex-basis to 0% automatically,
which is usually what you want. Writing the longhands separately can
cause unexpected cascading issues.

---

Q: What does align-self do and how does it differ from align-items?
A: align-self overrides the align-items value for one specific flex item,
allowing it to be aligned differently from its siblings. align-items is
set on the container and applies to all children; align-self is set on
an individual item.

---

Q: What does the order property do and what is its default value?
A: order controls the visual position of a flex item within the container.
The default is 0. Items are ordered from lowest to highest value. Items
with the same order value follow their source order. Important: this is
visual only — screen readers still read items in source order.

---

Q: What is the simplest way to perfectly center an element both horizontally and vertically using Flexbox?
A: Set display: flex on the parent, then set margin: auto on the child.
The auto margin absorbs all available space on every side, centering
the child on both axes. The parent needs a defined height for this to
be visible.

---

Q: What CSS properties have no effect on flex items?
A: float, clear, and vertical-align all have no effect on flex items.
Flexbox overrides these properties when applied to elements inside a
flex container.