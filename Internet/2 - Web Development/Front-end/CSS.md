CSS (Cascading Style Sheets) refers to the way styles are applied and can be overridden in a hierarchical manner. The idea is that styles can cascade down from one style sheet to another, creating a hierarchy of rules. The cascade is **the order of priority** that the browser follows when multiple styles are applied to the same element. 

"sheet" refers to a file or document that contains a set of styles to be applied to HTML or XML documents

In the cascade, styles are applied based on two main principles:
1. **Specificity:** When there are conflicting styles, more specific selectors have higher specificity and override less specific ones.
2. **Source Order:** When specificity is equal, styles declared later in the document or in a linked external style sheet take precedence over earlier styles.
3. **Important Rule:** Adding the `!important` declaration to a style rule gives it the highest priority, regardless of specificity or source order. But it is generally recommended to use `!important` sparingly, as it can make styles harder to maintain and debug.


## Layout
CSS layout can be broadly categorized into two types based on how elements interact with the normal flow of the document:
### 1. Normal Flow/Layout
Elements in the normal flow follow the default document layout.
- Block-level elements stack vertically, and inline elements flow horizontally within the content. e.g. `<div>`, `<p>`, `<h1>` to `<h6>`, `<span>`, etc.
### 2. Detached from Normal Flow/Layout
Elements that are taken out of the normal flow can be positioned independently.
#### Positioning (position property)
Elements can be positioned using properties like `position: absolute`, `position: fixed`, or `position: relative`.
#### Floats (float property)
Elements can be floated to the left or right, causing the surrounding content to flow around them.
#### FlexBox
The essence of Flexbox and the `flex` property is to simplify the design of complex and responsive user interfaces. It allows for the creation of layouts that adapt to different screen sizes and orientations while maintaining a clear and concise structure in the CSS code.
#### Grid
Provides a two-dimensional grid-based layout system where items can be placed independently in rows and columns.