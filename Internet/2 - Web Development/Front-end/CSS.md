## What is CSS?
CSS (Cascading Style Sheets) refers to the way styles are applied and can be overridden in a hierarchical manner. 
- Cascade is **the order of priority** when multiple styles are applied to the same element. 
- Sheets refers to a file or document that contains a set of styles applied to HTML/XML docs.

In the cascade, styles are applied based on two main principles:
1. **Specificity:** When there are conflicting styles, more specific selectors have higher specificity and override less specific ones.
2. **Source Order:** When specificity is equal, styles declared later in the doc or in a linked external style sheet take precedence over earlier styles.
3. **Important Rule:** The `!important` declaration has the highest priority regardless of specificity or source order. But it is generally recommended not to, as it can make styles harder to maintain and debug.

## Layout
CSS layout can be broadly categorized into two types based on how elements interact with the normal flow of the document:
### 1. Normal Flow/Layout
Elements in the normal flow follow the default document layout.
- Block-level elements stack vertically. e.g., `<div>`, `<p>`
- Inline elements flow horizontally within the content. e.g. `<div>`, `<p>`, `<h1>`, `<span>`, etc.
- Inline-Block Elements: Behave like inline elements but maintain block-level features regarding width and height.
### 2. Detached from Normal Flow/Layout
Elements that are taken out of the normal flow can be positioned independently.
#### Positioning (position property)
Elements can be positioned using properties like `position: absolute`, `position: fixed`, or `position: relative`.
#### Floats (float property)
Elements can be floated to the left or right, causing the surrounding content to flow around them.

#### FlexBox
Lay the elements along an axis. The main axis is default horizontal (justify-content) and cross axis is default vertical (align-items). 
- justify-content: Aligns flex items along the main axis(default horizontal).
- align-items: Aligns flex items along the cross axis(default vertical).
- flex-direction: Define the direction of the main axis. `row(default), row-reverse, column, column-reverse`
- order: Specifies the order of the flex item.
- align-self: Aligns a flex item along the cross axis, overriding the align-items value. `flex-start, flex-end, center, baseline, stretch`
- flex-wrap: Specifies whether flex items are forced on a single line or can be wrapped on multiple lines. `nowrap(default), wrap, wrap-reverse`
- flex-flow: flex-direction + flex-wrap.
- align-content: Aligns a flex container's lines within the flex container when there is extra space on the cross-axis, `justify-content` 的multiple items版本. `flex-start, flex-end, center, space-between, space-around, space-evenly, stretch(default)`

![[CSS-Flexbox-froggy-24.png]]

#### Grid
Provides a two-dimensional grid-based layout system where items can be placed independently in rows and columns.
- grid-template: specify the sizing and names of the rows and columns.
- grid-template-rows: specify the sizing and names of the rows.
- grid-template-columns: specify the sizing and names of the columns.
![[CSS-Grid-garden-26.png]]

![[CSS-Grid-garden-28.png]]


## Responsive Web Design
Use CSS and HTML to resize, hide, shrink, enlarge, or move the content to make it look good on any screen.
### Viewpoint
The user's visible area of a web page.
### Media Query
Introduced in CSS3, it uses the `@media` rule to include a block of CSS properties only if a certain condition is true.

## Modern CSS / Utility CSS
Utility first, which is to apply styles directly in HTML using utility classes. 
- More flexible compared with the traditional CSS.
- A giant CSS design system that can maintain consistency through the project.

| class | properties        |
| ----- | ----------------- |
| p-4   | padding: 1rem;    |
| p-5   | padding: 1.25rem; |

```html
// React example with Tailwind CSS
const Button = ({ children, variant }) => {
  return (
    <button className={`px-4 py-2 border-0 cursor-pointer ${variant}`}>
      {children}
    </button>
  );
};

// Usage
<Button variant="bg-blue-500 text-white">Click me</Button>

```

**Example:** Tailwind, Radix UI, Shadcn UI
- Tailwind: CSS Framework providing atomic CSS classes to style components e.g. `flex`, `pt-4`, `text-center`, `rotate-90` that can be composed to build any design, directly in markup.


## CSS Architecture
Manage CSS in large, complex systems.
- **Object-Oriented CSS (OOCSS):** Separates structure from skin, focusing on reusing patterns.
- **BEM:** The Block, Element, Modifier methodology: Encourages a structured naming convention for classes in HTML and CSS, enhancing readability and reusability.
- **Scalable and Modular Architecture for CSS (SMACSS):** A modular architecture, categorizing CSS rules for better flexibility and maintainability.
- **SUIT CSS:** Provides a naming convention and methodology focused on component-based development.
- **Atomic CSS:** Advocates for single-purpose classes based on visual functionality.



