The [e18e CLI](https://github.com/e18e/cli) has a super useful `analyze` mode to determine which dependencies are no longer needed, or have community recommended replacements.

For example, if you get something like this:

[npmgraph](https://npmgraph.js.org) is a great tool to visualize your dependency tree and investigate where bloat is coming from.



This is **highly accurate** and aligns perfectly with the **Fail-Fast Principle** and **Defensive Programming**.
In software engineering, it is almost always better for an application to crash loudly and immediately than to continue running in an unpredictable state. Throwing an exception in the `default` block guarantees that an unhandled case is caught the very first time that code path is executed.


DRY is about isolating _knowledge_, not just deduplicating code.

The biggest mistake junior to mid-level developers make is thinking DRY means: _"If I see two identical lines of code, I must combine them."_

This leads to the **Wrong Abstraction** anti-pattern. You should only combine code if it represents the **exact same business concept** and will change for the **exact same reason**.

magine your e-commerce app has a `ProductDisplay` component and a `UserProfile` component. By pure coincidence, both components have a UI card with a blue border, a shadow, and a 16px font.

- A developer strictly applying DRY might combine them into a `UniversalCard` component used by both the Product and the User Profile.
    
- A month later, the designer says: _"We want Product cards to turn green when on sale."_
    
- Now, the developer has to add a messy `if (isProduct)` flag inside the `UniversalCard` to prevent User Profiles from turning green.