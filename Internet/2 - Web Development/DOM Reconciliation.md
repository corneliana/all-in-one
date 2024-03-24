The core technology behind DOM Reconciliation is the **Virtual DOM**.

### Virtual DOM:
- A lightweight, virtual and in-memory representation of the UI in sync with the real DOM.

### Key Tech in Virtual DOM Reconciliation:
- **Diffing Algorithm**: React compares the previous and next versions of the Virtual DOM to identify what has changed. It aims to perform comparisons while minimizing the updates to the actual DOM, as DOM operations are expensive.
- **Reconciliation**: Once React identifies the differences between the old and new Virtual DOM trees, it reconciles the differences by updating the actual DOM by:
    - **Batching** multiple updates together to minimize direct manipulations of the actual DOM, improving performance.
    - **Update Prioritization**: Prioritize updates (e.g., user inputs or animations) that need to be more immediate, delaying less critical updates to ensure a smooth user experience.
- **Components and State Management**: React's component-based architecture and state management play a critical role. When a component's state changes, React triggers a re-render, leading to a new Virtual DOM tree that will be compared against the previous one. This selective rendering mechanism ensures that only the components that need updates are re-rendered, enhancing performance.
