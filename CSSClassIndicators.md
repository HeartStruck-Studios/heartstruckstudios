# CSS Selectors & Rules (Beginner Reference)

This document is a beginner-friendly guide to CSS selectors, what they target, how they work, and the rules that decide which styles override others.

IMPORTANT Markdown note (GitHub):
- To show code neatly without using triple-backtick code blocks, indent code lines by 4 spaces.
- Symbols like `*` or `>` should be written in inline code to display properly.

---

# CSS Syntax Basics

CSS is written like this:

    selector {
      property: value;
    }

Example:

    p {
      color: red;
    }

Meaning:
- Select all `<p>` elements
- Change their text colour to red

---

# 1) Core CSS Selectors

---

## Universal Selector (`*`)

What it targets:
- Every single element on the page.

Example:

    * {
      box-sizing: border-box;
    }

Why it’s used:
- Global resets and defaults
- Predictable sizing across the page

Be careful:
- Heavy styling here affects everything and can make debugging harder.

---

## Element / Type Selector (`p`, `div`, `h1`, etc.)

What it targets:
- All elements of a given HTML tag.

Example:

    p {
      font-size: 16px;
      line-height: 1.6;
    }

Best for:
- Global typography rules
- Consistent defaults

Avoid:
- Over-styling very broad tags like `div`

---

## Class Selector (`.className`)

What it targets:
- Any element with that class attribute.

HTML:

    <button class="btn">Save</button>
    <button class="btn">Submit</button>

CSS:

    .btn {
      padding: 10px 14px;
      border-radius: 8px;
    }

Why classes matter:
- Reusable across many elements
- Most common way to style components

Rule:
- If you will reuse the style, use a class.

---

## ID Selector (`#idName`)

What it targets:
- One unique element with that ID.

HTML:

    <header id="site-header">Welcome</header>

CSS:

    #site-header {
      background: black;
      color: white;
    }

Rules:
- IDs must be unique on a page
- Use for one-off layout sections or anchor links

Avoid:
- Using IDs for repeated styling

---

# 2) Combining Selectors (Targeting More Specifically)

Combining selectors lets you target elements based on where they are inside other elements.

This is extremely common for menus, cards, layouts, and structured pages.

---

## Descendant Selector (space)

Written like:

    .menu a

What it targets:
- Any `<a>` element anywhere inside an element with class `.menu`

Example HTML:

    <nav class="menu">
      <ul>
        <li><a href="#">Home</a></li>
      </ul>
    </nav>

Example CSS:

    .menu a {
      text-decoration: none;
      color: blue;
    }

Meaning:
- All links inside `.menu` are styled
- This includes nested links, no matter how deep they are

Why it’s useful:
- Style all links inside a navigation area
- Style all paragraphs inside an article
- Style all buttons inside a card

---

## Direct Child Selector (`>`)

Written like:

    .menu > a

What it targets:
- Only elements that are DIRECT children of the parent
- It does NOT select deeper nested elements

Think of it like:
- Space selector = "anywhere inside"
- `>` selector = "immediately inside"

Example HTML:

    <nav class="menu">
      <a href="#">Top Link</a>

      <div class="dropdown">
        <a href="#">Dropdown Link</a>
      </div>
    </nav>

Example CSS:

    .menu > a {
      font-weight: bold;
    }

Result:
- "Top Link" becomes bold (direct child)
- "Dropdown Link" does NOT (because it is inside `.dropdown`, not directly inside `.menu`)

Another common example with lists:

HTML:

    <ul class="list">
      <li>Main item 1</li>
      <li>
        Main item 2
        <ul>
          <li>Nested item</li>
        </ul>
      </li>
    </ul>

CSS:

    .list > li {
      margin: 10px 0;
    }

Result:
- Only the top-level list items get margin
- Nested list items stay unaffected

Why it’s useful:
- Prevent styling from accidentally applying to nested content
- Style only the first level of a menu or list

---

## Group Selector (comma)

Applies the same rule to multiple selectors.

Example:

    h1, h2, h3 {
      font-family: sans-serif;
    }

---

# 3) Attribute Selectors

Attribute selectors target elements based on HTML attributes.

---

## Attribute Exists (`[attr]`)

Example:

    a[target] {
      text-decoration: underline;
    }

Meaning:
- Any link with a `target` attribute is underlined

---

## Attribute Equals Value (`[attr="value"]`)

Example HTML:

    <input type="email">
    <input type="password">

Example CSS:

    input[type="email"] {
      border: 2px solid green;
    }

Meaning:
- Only email inputs get styled

Other useful examples:

    button[disabled] {
      opacity: 0.5;
    }

    [data-state="open"] {
      display: block;
    }

---

# 4) Pseudo-Classes (States)

Pseudo-classes style elements when they are in a certain state.

---

## Hover (`:hover`)

Applies when the mouse is over an element.

Example:

    .btn:hover {
      opacity: 0.85;
    }

Use for:
- Interactive feedback
- Highlighting buttons/links

---

## Focus (`:focus`)

Applies when an element is selected (keyboard or click).

Example:

    input:focus {
      outline: 2px solid black;
    }

Why it matters:
- Accessibility for keyboard users
- Important for form usability

---

## Nth Child (`:nth-child(n)`)

Selects elements by their position among siblings.

Example (2nd item):

    li:nth-child(2) {
      font-weight: bold;
    }

Example (odd items):

    li:nth-child(odd) {
      opacity: 0.9;
    }

Example (every 3rd):

    li:nth-child(3n) {
      text-decoration: underline;
    }

Important:
- It counts all children, not just matching tags.

---

# 5) Pseudo-Elements (Parts of Elements)

Pseudo-elements let you style parts of an element or insert decorative content.

---

## `::before` and `::after`

They create a “virtual” element inside the target.

Example:

    .tag::before {
      content: "#";
      margin-right: 6px;
    }

Example: add separators:

    .nav a::after {
      content: " |";
      opacity: 0.4;
    }

Example: display attribute text:

HTML:

    <li class="item" data-detail="Extra info">Item</li>

CSS:

    .item::after {
      content: " — " attr(data-detail);
      opacity: 0.7;
    }

---

# 6) Specificity & Strength Order (Why CSS Overrides Other CSS)

When multiple CSS rules target the same element, the browser decides which rule wins based on:

1. Specificity (how targeted the selector is)
2. Source order (which rule comes later)
3. `!important` overrides

---

## Strength Order (Weak → Strong)

1. Element selectors (`p`, `div`)
2. Class selectors (`.box`), attribute selectors (`[type="text"]`), pseudo-classes (`:hover`, `:focus`, `:nth-child`)
3. ID selectors (`#main`)
4. Inline styles (`style=""`)
5. `!important` rules

---

## Element Selectors (Weakest)

Example:

    p {
      color: blue;
    }

Easy to override with a class:

    .text {
      color: red;
    }

---

## Class / Attribute / Pseudo-Class Selectors (Medium)

Example:

    .card {
      padding: 20px;
    }

Attribute example:

    input[type="email"] {
      border: 2px solid green;
    }

Pseudo-class example:

    button:hover {
      opacity: 0.8;
    }

These override element selectors.

---

## ID Selectors (Very Strong)

Example:

    .box {
      background: blue;
    }

    #box1 {
      background: red;
    }

ID wins over class.

---

## Inline Styles (Extremely Strong)

Example:

    <p style="color: green;">Hello</p>

Inline overrides nearly all stylesheet rules.

Avoid inline styling for maintainability.

---

## `!important` (Strongest Override)

Example:

    p {
      color: blue !important;
    }

Even an ID will not override it:

    #intro {
      color: red;
    }

Why it’s dangerous:
- Breaks the normal cascade
- Makes overrides difficult
- Leads to messy CSS

Acceptable uses:
- Overriding third-party libraries
- Utility classes like:

    .hidden {
      display: none !important;
    }

---

## Source Order Matters Too

If specificity is equal, the later rule wins:

    .box {
      color: blue;
    }

    .box {
      color: red;
    }

Result: red wins.

---

# 7) Bullet Points With Extra Text (No Extra Bullet)

HTML:

    <ul>
      <li>
        # = ID selector
        <small class="extra">Targets one unique element using its ID attribute.</small>
      </li>
    </ul>

CSS:

    .extra {
      display: block;
      margin-left: 1.2em;
      font-size: 0.9em;
      opacity: 0.8;
    }

---

# 8) Common HTML Mistake (Self-Closing Tags)

Incorrect:

    <small class="extra"/>

Correct:

    <small class="extra">Text goes here</small>

---

# 9) Best Practices

- Use classes for reusable styling
- Use IDs only for unique sections
- Keep selectors simple and readable
- Avoid inline styles
- Avoid `!important` unless necessary
- Use Flexbox or Grid for layout instead of random margins
