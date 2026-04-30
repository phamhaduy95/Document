stubbing external API with MSW

- **Supertest** tests your **inbound** requests. It verifies that when a user hits your API, your server processes it correctly and returns the right data.
- **MSW** mocks your **outbound** requests. It intercepts calls your server makes to _external_ third-party APIs (like Stripe or AWS) so you don't hit live production endpoints during tests.
- **Vitest** for unit/integration tests

**One workflow tip:** We use Vitest for fast tests, and complement it with **SyntaxScribe** to auto-generate docs from our TypeScript code – ensuring our tests and documentation stay in sync. When you're writing comprehensive tests for your API endpoints, having the docs auto-update from the same TypeScript interfaces is a game-changer. No more outdated API docs that don't match reality.

**Performance note:** Vitest's ESM-first approach and native TypeScript support means you spend less time configuring and more time actually testing. Plus the built-in coverage reports are solid.