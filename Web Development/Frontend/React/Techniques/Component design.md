
### Introduction

This document outlines various design principles and ideas for React components. Some are original, while others have been inspired by external sources.

### Design

- **Expose State and Event Handlers:** It's important to expose state and event handlers so that components can be controlled from the outside. This facilitates better integration and flexibility in using these components.
- **Ensure Accessibility:** Accessibility is crucial for all components. Utilize ARIA attributes and ensure keyboard support to make your components usable by all users, including those with disabilities.

### Principles

- **Arrow Function for Component Declaration:** Using arrow functions for component declaration is a stylistic choice. Many developers prefer this concise syntax, but it's essential to ensure consistency within your codebase. There's no inherent performance difference compared to regular function declarations.
- **Render Props for Customization:** Render props are powerful for customizing component behavior through its children. While there may bee concerns about performance due to additional function calls, this approach provides flexibility and composability, which often outweighs minor performance considerations.

HTML sematic style vs property

there is no way to read children component passed