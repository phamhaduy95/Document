By default, named paths defined in `tsconfig.json` aren't usable unless specified in the `define` property of `vite.config`. To simplify this, you can use the `vite-tsconfig-paths` plugin, which only requires settings in the `tsconfig.json` file.

```tsx
import { defineConfig } from 'vite';
import path from 'path';

export default defineConfig({
	resolve: {
		alias: {
			'@components': path.resolve(__dirname, './src/components'),
			'@utils': path.resolve(__dirname, './src/utils'),
		},
	},
});
```

```json
{
    "jsx": "react-jsx",
    "paths": {
      "src/*":["./src/*"],
      "@components": ["./src/Components"],
      "@components/*":["./src/Components/*"] ,
      "@styles/*":["./src/styles/*"],
      "@model/*":["./src/models/*"],
      "@pages/*":["./src/pages/*"],
}
```

```tsx
import { defineConfig } from 'vite';

// vite-tsconfig-paths plugin allows directing import via alias pathname defined in tsconfig
// for example : import * from @component
import tsconfigPaths from 'vite-tsconfig-paths';

export default defineConfig({
    plugins: [tsconfigPaths()],
});
```

Using the plugin has a significant limitation: it only includes imports that adhere to predefined paths in `tsconfig`. Direct imports are excluded from the final built bundle in both development and production modes.

```tsx
// fail 
import Button from 'src/Components/Button'; 

```

## 💡Idea

Create new plugin to help resolve file import path for `vite`