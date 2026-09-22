HTML Interview Questions to prepare mock interview 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. What is HTML?
~~~~~~~~~~~~~~~~

Stands for HyperText Markup Language
Standard markup language for structuring web content
Not a programming language — no logic, just structure

2. Purpose of <!DOCTYPE html>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Tells the browser to use standards mode, not quirks mode
Quirks mode = old compatibility rendering, inconsistent behavior
HTML5 simplified it to just <!DOCTYPE html> 

3. Basic structure of an HTML document
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

<!DOCTYPE html> declaration first
<html> — root element
<head> — metadata (title, charset, links to CSS)
<body> — visible page content

4. Tag vs. Element
~~~~~~~~~~~~~~~~~~

Tag = the syntax itself, e.g. <p> or </p>
Element = full unit — opening tag + content + closing tag, e.g. <p>Hello</p>

5. Attributes
~~~~~~~~~~~~~

Provide extra info about an element
Written as name="value" inside the opening tag
Example: href and target in <a href="..." target="_blank">

6. Block-level vs. Inline elements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Block-level: starts on new line, takes full width — div, p, h1, ul, section
Inline: stays in line, only takes needed width — span, a, strong, em

7. Void elements
~~~~~~~~~~~~~~~~

No content, no closing tag needed
Examples: img, br, hr, input, meta, link

8. Semantic HTML
~~~~~~~~~~~~~~~~

Uses tags that describe meaning, not just appearance
Important for accessibility (screen readers) and SEO
Examples: header, nav, main, article, section, footer

9. id vs. class
~~~~~~~~~~~~~~~

id — unique, one element per page
class — reusable, multiple elements/classes allowed
id for single targets, class for grouping/styling

10. Hyperlinks & URLs
~~~~~~~~~~~~~~~~~~~~~

Created with <a href="...">
Absolute URL — full address incl. domain
Relative URL — path relative to current page location

11. Opening a link safely in a new tab
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use target="_blank"
Add rel="noopener noreferrer" for security
Prevents new page from accessing window.opener

12. Form tag
~~~~~~~~~~~~

<form> collects user input
action — URL data is sent to
method — HTTP method used (GET/POST)

13. img tag & alt
~~~~~~~~~~~~~~~~~

src — image file path
alt required for: accessibility (screen readers), fallback if image fails, SEO

14. HTML vs. HTML5
~~~~~~~~~~~~~~~~~~

HTML5 added: semantic tags, native audio/video, canvas, new form input types
Also added APIs: localStorage, geolocation, drag-and-drop
Simplified doctype and parsing rules

15. Meta tags & SEO
~~~~~~~~~~~~~~~~~~~

Provide metadata not shown on page, read by browsers/search engines
description tag often shown in search snippets
viewport tag affects mobile rendering and ranking

16. localStorage vs. sessionStorage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Both store client-side key-value data
localStorage — persists until manually cleared
sessionStorage — cleared when tab closes

17. HTML entities
~~~~~~~~~~~~~~~~~

Represent special characters using codes
Start with &, end with ;
Examples: &lt;, &gt;, &amp;, &nbsp;

18. div vs. section
~~~~~~~~~~~~~~~~~~~

div — generic, non-semantic container for layout/styling
section — semantic, represents thematic content grouping

19. data- attributes*
~~~~~~~~~~~~~~~~~~~~~

Custom attributes for storing extra data on elements
Accessed via JS using dataset
Used for app-specific data without misusing class/id

20. script vs. async vs. defer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

script — blocks parsing, downloads & runs immediately
async — downloads in parallel, runs as soon as ready (order not guaranteed)
defer — downloads in parallel, runs after parsing, in order

21. Accessibility / ARIA
~~~~~~~~~~~~~~~~~~~~~~~~

Use semantic HTML first
Add ARIA roles/attributes only when needed (aria-label, aria-expanded)
Ensure heading structure, keyboard navigation, alt text, color contrast

22. SVG vs. Canvas
~~~~~~~~~~~~~~~~~~

SVG — vector-based, DOM elements, scalable, CSS-stylable, good for icons/diagrams
Canvas — pixel-based, JS-drawn bitmap, better for performance-heavy/dynamic graphics

23. picture element
~~~~~~~~~~~~~~~~~~~

Lets browser pick best image source based on conditions (screen size, format)
Uses multiple <source> tags + fallback <img>
Used for responsive/art-directed images

24. GET vs. POST
~~~~~~~~~~~~~~~~

GET — data in URL, visible, cacheable, size-limited, for non-sensitive requests
POST — data in request body, not visible, no size limit, for sensitive/state-changing actions

25. Shadow DOM
~~~~~~~~~~~~~~

Attaches hidden, encapsulated DOM tree to an element
Styles/markup scoped, don't leak in or out
Core to Web Components — enables reusable, isolated custom elements

--------------------------------------------------------------------------------------------------

1. What is HTML?
"HTML stands for HyperText Markup Language. It's the standard markup language for creating web pages — it defines the structure and content of a page, like headings, paragraphs, links, and images. It's not a programming language since it has no logic, just structure."

2. Purpose of <!DOCTYPE html>
"It tells the browser to render the page in standards mode instead of quirks mode, which is an old compatibility mode. In HTML5 it's simplified to just <!DOCTYPE html>, no version number needed."

3. Basic structure
"Every HTML document starts with the doctype, then an <html> root element containing a <head> for metadata like title and charset, and a <body> for the visible content."

4. Tag vs Element
"A tag is just the syntax — like <p> or </p>. An element is the full thing: opening tag, content, and closing tag together, like <p>Hello</p>."

5. Attributes
"Attributes give extra information about an element and go inside the opening tag as name-value pairs — like href in <a href="...">. They control behavior or provide metadata without adding visible content."

6. Block vs Inline
"Block-level elements start on a new line and take up the full width — like div, p, h1. Inline elements only take up as much space as needed and don't break the line — like span, a, strong."

7. Void elements
"Void elements are tags that don't have content or a closing tag, since there's nothing to wrap — like img, br, hr, and input."

8. Semantic HTML
"Semantic HTML means using tags that describe their meaning, not just their appearance — like header, nav, article, footer instead of generic divs. It matters for accessibility, since screen readers rely on it, and for SEO."

9. id vs class
"id is unique — only one element per page can have it, so it's used for a specific single target. class can be reused across multiple elements and one element can have several classes — it's for grouping and shared styling."

10. Hyperlinks & URLs
"You create a link with the a tag and href. An absolute URL is the full address including the domain, while a relative URL is just the path relative to the current page — so it depends on file location."

11. Opening a link safely in a new tab
"You use target="_blank" to open in a new tab, and add rel="noopener noreferrer" — that's important for security, because without it the new page could access window.opener and potentially redirect your original tab."

12. Form tag
"The form tag collects user input. action is the URL the data gets sent to, and method defines how — usually GET or POST."

13. img tag & alt
"img embeds an image using src for the file path. alt is required — it's used by screen readers for accessibility, shows if the image fails to load, and helps with SEO."

14. HTML vs HTML5
"HTML5 added semantic tags, native audio and video support without needing plugins like Flash, the canvas element for graphics, new form input types, and APIs like localStorage and geolocation. Overall it's more capable and standardized."

15. Meta tags & SEO
"Meta tags provide metadata that isn't visible on the page but is read by browsers and search engines — like the description tag, which often shows up in search snippets, and viewport, which affects mobile rendering and ranking."

16. localStorage vs sessionStorage
"Both store key-value data client-side. localStorage persists even after the browser closes, until it's manually cleared. sessionStorage only lasts for that tab's session — it clears when the tab closes."

17. HTML entities
"Entities represent special characters using a code that starts with & and ends with ; — like &lt; for < or &amp; for &. You need them because those characters would otherwise be interpreted as markup."

18. div vs section
"div is a generic, non-semantic container just for grouping or styling. section is semantic — it represents a thematic grouping of content, usually with its own heading, and it actually conveys meaning to the browser and assistive tech."

19. data- attributes*
"They're custom attributes you can add to store extra data on an element, accessible through JavaScript via dataset. They're useful for attaching app-specific info without misusing classes or ids."

20. script vs async vs defer
"A normal script tag blocks HTML parsing while it downloads and runs. async downloads in parallel and executes as soon as it's ready, which can interrupt parsing — execution order isn't guaranteed. defer also downloads in parallel but waits until parsing is done, and runs scripts in order."

21. Accessibility / ARIA
"Start with semantic HTML since it's accessible by default. Use ARIA roles and attributes — like aria-label or aria-expanded — only when native HTML can't express something. Also make sure there's proper heading structure, keyboard navigation, and alt text."

22. SVG vs Canvas
"SVG is vector-based — each shape is its own DOM element, so it scales without losing quality and can be styled or animated with CSS. Canvas is pixel-based — you draw on a bitmap with JavaScript, which is better for performance-heavy or dynamic graphics like games, but individual shapes aren't interactive on their own."

23. picture element
"picture lets the browser choose the best image source based on conditions like screen size or format support, using multiple source tags with a fallback img. It's mainly for responsive and art-directed images."

24. GET vs POST
"GET sends data through the URL as query parameters — visible, cacheable, and size-limited, so it's used for things like search. POST sends data in the request body, so it's not visible in the URL and has no real size limit — used for sensitive data or anything that changes server state."

25. Shadow DOM
"Shadow DOM lets you attach a hidden, encapsulated DOM tree to an element, with styles and markup scoped so they don't leak in or out. It's core to Web Components — it lets you build reusable custom elements whose internals are isolated, similar to how a native video player's controls are hidden from the main page."
