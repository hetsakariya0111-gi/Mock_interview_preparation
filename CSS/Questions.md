CSS Interview Questions to prepare mock interview 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. CSS Box Model
~~~~~~~~~~~~~~~~

Describes how every element is structured as a rectangular box
Core components (inside out): content → padding → border → margin
Content: actual text/image
Padding: space between content and border
Border: edge surrounding the padding
Margin: space outside the border, between elements

2. content-box vs. border-box
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

content-box (default): width/height applies only to content — padding and border add extra size on top
border-box: width/height includes content, padding, and border — total size stays fixed
border-box is generally preferred for predictable, easier layout sizing

3. Selector Specificity
~~~~~~~~~~~~~~~~~~~~~~~

Determines which CSS rule wins when multiple rules target the same element
Calculated by weight: inline styles > IDs > classes/attributes/pseudo-classes > elements/pseudo-elements
Higher specificity wins; if equal, the rule that comes later in the source wins
!important overrides specificity entirely (with caveats)

4. block vs. inline vs. inline-block
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

block — starts new line, takes full width, respects width/height/margin/padding
inline — stays in line, only as wide as content, ignores width/height, vertical margin/padding limited
inline-block — stays in line like inline, but respects width/height/margin/padding like block

5. CSS position values
~~~~~~~~~~~~~~~~~~~~~~

static — default, normal document flow, offsets (top/left etc.) ignored
relative — positioned relative to its own normal position, offsets apply
absolute — positioned relative to nearest positioned ancestor (or document if none), removed from normal flow
fixed — positioned relative to the viewport, stays fixed on scroll
sticky — toggles between relative and fixed based on scroll position within its container

6. Centering a div horizontally and vertically
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Method 1 — Flexbox:
css
  .parent { display: flex; justify-content: center; align-items: center; }
Method 2 — Grid:
css
  .parent { display: grid; place-items: center; }
Method 3 — Absolute positioning + transform:
css
  .child { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); }

7. Flexbox, justify-content & align-items
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Flexbox = one-dimensional layout system for arranging items in a row or column
justify-content — aligns items along the main axis (e.g. horizontally in a row)
align-items — aligns items along the cross axis (e.g. vertically in a row)
Common values: flex-start, center, space-between, space-around

8. Flexbox vs. Grid
~~~~~~~~~~~~~~~~~~~

Flexbox — one-dimensional (row OR column), best for aligning items in a single line/direction, like navbars or button groups
Grid — two-dimensional (rows AND columns), best for full page layouts or complex grid structures
Rule of thumb: Flexbox for components, Grid for overall layout

9. display: none vs. visibility: hidden
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

display: none — element is removed from layout entirely, takes up no space
visibility: hidden — element is hidden but still takes up its space in the layout
Both hide the element visually, but affect layout differently

10. Absolute (px) vs. relative (em, rem, %) units
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

px — fixed, absolute unit, doesn't scale with anything
em — relative to the font-size of the parent element (compounds if nested)
rem — relative to the font-size of the root (html) element — avoids compounding issues
% — relative to the parent element's corresponding property (e.g. width)
Relative units are preferred for responsive, accessible design

11. Media Queries
~~~~~~~~~~~~~~~~~

CSS technique to apply styles conditionally based on device characteristics
Common conditions: screen width, height, orientation, resolution
Example:
css
  @media (max-width: 768px) { .nav { display: none; } }
Core tool for responsive design — adapts layout across screen sizes

12. z-index & stacking context
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

z-index controls the stack order of positioned elements (which sits on top)
Only works on elements with a position value other than static
Stacking context — a conceptual "layer" grouping; z-index values only compare within the same stacking context
New stacking contexts are created by things like position + z-index, opacity < 1, transform, etc.

13. Pseudo-classes vs. Pseudo-elements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Pseudo-classes — target elements in a specific state: :hover, :focus, :nth-child()
Pseudo-elements — target a specific part of an element: ::before, ::after, ::first-line
Syntax difference: single colon for pseudo-classes, double colon for pseudo-elements (convention in CSS3)

14. !important rule
~~~~~~~~~~~~~~~~~~~

Forces a style to override any other declaration, regardless of specificity
Discouraged because: breaks the natural cascade, makes debugging harder, leads to specificity wars, reduces maintainability
Better alternatives: increase specificity properly or restructure CSS

15. CSS Variables (Custom Properties)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Reusable values defined with --variable-name and accessed via var(--variable-name)
Example:
css
  :root { --primary-color: #3498db; }
  .btn { color: var(--primary-color); }
Useful for: theming, consistency, easy global updates, can be scoped and even changed via JavaScript