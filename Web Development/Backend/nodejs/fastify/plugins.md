

By default, `register` creates a _new scope_, meaning changes to the Fastify instance (via `decorate`) will not affect the current context ancestors, only its descendants. This feature enables plugin _encapsulation_ and _inheritance_

