React is actually a JavaScript library for building user interfaces with **Component** as its core.
A component is a piece of UI that has its own logic and appearance. In implementation, a component is a JS class / function that has some state and a render method and return markup.

## Hooks
### useContext
provides a convenient way to share data across the component tree. We can access the value of a context and subscribe to its changes. But overusing it can lead to less modular and harder-to-manage code. 

when should we use useContext?
Best use it for data that is truly global and used across many parts of application, or the scope is limited and prop drilling is manageable.

### useReducer