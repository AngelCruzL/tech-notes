---
tags: guide frontend
---
-----

# React with Typescript

First we need to install the next packages:
```bash
yarn add @mui/material @emotion/react @emotion/styled -E
```

On `tsconfig.json` file we need to add the next content:
```json
{
	"compilerOptions": {
		"lib": ["es6", "dom"],
		"noImplicitAny": true,
		"noImplicitThis": true,
		"strictNullChecks": true
	}
}
```

If you want to make some customization to some color or option provided by the default theme behavior, you can see the docs, but in resume you can create some file on root project (due to architectural order I’ll create this file inside of one dir named _****theme****_)

Inside my `theme/customTheme` you can see the next:

```tsx
import { createTheme, ThemeOptions } from '@mui/material';

export const customTheme: ThemeOptions = createTheme({
  palette: {
    mode: 'dark',
    primary: {
      light: 'rgba(168,85,247,.80)',
      main: 'rgba(168,85,247,.65)',
      dark: 'rgba(168,85,247,.28)',
    },
    background: {
      paper: '#151515',
      default: 'rgba(0,0,0,.96)',
    },
  },
});
```

Here I’m overriding the default mode of the theme to `dark` and overriding some colors, by default the no overrited properties will be not affected and will have the default behavior.

Once we had the customTheme we need to provide it on root application, on `App.tsx`:

```tsx
import { FC, ReactElement } from 'react';
import { CssBaseline, ThemeProvider } from '@mui/material';

import { customTheme } from './theme/customTheme';

const App: FC = (): ReactElement => {
  return (
    <ThemeProvider theme={customTheme}>
      <CssBaseline />
      
      <div>Hello World</div>
    </ThemeProvider>
  );
};

export default App;
```

Here we to provide the customTheme to the custom component named ThemeProvider, also, we’re using the CssBaseline component to reset the navigator’s default styles.