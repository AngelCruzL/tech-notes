---
tags: guide frontend
---
----

# Set up vite paths

[https://www.npmjs.com/package/vite-tsconfig-paths](https://www.npmjs.com/package/vite-tsconfig-paths)

This will be enable the path aliases on typescript projects based on vite bundler.

First we need to install the next package:
```bash
yarn add -E vite-tsconfig-paths
```


Now on the `vite.config.ts` file we need to add the next content:
```ts
import { defineConfig } from 'vite'
import tsconfigPaths from 'vite-tsconfig-paths'

export default defineConfig({
	plugins: [tsconfigPaths()],
})
```


# Vitest setup

First we need to install the next packages:
```bash
yarn add -DE vitest jsdom
```

I separated the files because i got the from this question and if i changed the import to vitest i got another error in the plugin react line.

- vite.config.ts
- vitest.config.ts


vitest:
```json
import { defineConfig } from 'vitest/config';

export default defineConfig({
	test: {
		globals: true,
		environment: 'jsdom',
	},
});
```


vite:
```json
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
	plugins: [react()],
	server: {
		port: 3000,
	},
})
```


`tsconfig.json`:
```tsx
{
	compilerOptions: {
		"types": ["vitest/globals"]
	}
}
```



## Add react testing library
[https://testing-library.com/docs/react-testing-library/intro](https://testing-library.com/docs/react-testing-library/intro)

We need to install the next packages
```bash
yarn add -DE @testing-library/react
```