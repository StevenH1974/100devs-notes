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