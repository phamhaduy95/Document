This guide will teach us to how to import SVG file as React Component with `vite` 

Although, `vite` supports SVG import out of the box, the SVG file is treated as URL. To be able to use it as React Component, follow these steps below.

1. Install `vite-plugin-svgr`

```bash
# with yarn
yarn add -D vite-plugin-svgr

#with npm
npm i -D vite-plugin-svgr
```

1. Add `vite-svgr-plugin` into `vites.config.ts`

```tsx
import { defineConfig } from 'vite';

import react from '@vitejs/plugin-react';
import svgr from 'vite-plugin-svgr';

import tsconfigPaths from 'vite-tsconfig-paths';

export default defineConfig({
    plugins: [react(), svgr(), tsconfigPaths()],

    css: {
        modules: {
            localsConvention: 'camelCaseOnly',
        },
    },
    cacheDir: '/.vite',
});
```

1. Provide type reference inside `vite-env.d.ts`.

```tsx
/// <reference types="vite-plugin-svgr/client" />
/// <reference types="vite/client" />
```