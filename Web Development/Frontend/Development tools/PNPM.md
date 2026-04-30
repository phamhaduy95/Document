#### Collection of useful commands from pnpm 
``` bash
pnpm prune --production
```

The command removes any package that is not listed in `dependency` field from `package.json`, which would reduce total size of the final bundle.

```bash
pnpm store prune
```

The command clears any cached package that does not have reference in the project. This command should be done periodically to save local disc space.


All major JavaScript frameworks are [component-driven](https://www.componentdriven.org/). That means UIs are built from the “bottom-up”, starting with atomic components and progressively composed into pages.