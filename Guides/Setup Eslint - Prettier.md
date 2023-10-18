---
tags: guide frontend backend
---
----

# Frontend

## Eslint

First we need to install the next packages:
```bash
yarn add eslint eslint-plugin-import eslint-import-resolver-typescript -DE
```
This command install the dependencies at the last exact version and as development dependencies.


Now we need to start the eslint configuration with the next command:
```bash
npx eslint --init
```

Now we need to follow the CLI to set up as we prefer the eslint configuration. After select the options this will generate a file for the configs (in my case I prefer to choose the `json` version), and we add the next:
```json
{
	...
	"settings": {
	    "import/resolver": {
		    "typescript": {}
	    }
	}
	...
}
```


## Prettier

Now we need to make the configurations to add prettier support to our default eslint rules. Now we need to install the next packages:

```bash
yarn add -DE prettier eslint-config-prettier eslint-plugin-prettier
```

And now we create a `.prettierrc` file with the next content:
```json
{  
  "arrowParens": "avoid",  
  "bracketSpacing": true,  
  "printWidth": 80,  
  "semi": true,  
  "singleQuote": true,  
  "tabWidth": 2,  
  "trailingComma": "all"  
}
```


And now we make some changes on our eslint configurations file:
```json
{
	...
	"extends": [
		"eslint:recommended",
		"plugin:react/recommended",
		"plugin:@typescript-eslint/recommended",
		"prettier"
	],
	...
	"plugins": [
		"react",
		"@typescript-eslint",
		"prettier"
	],
	...
}
```


If you have the error _**“ESLint: 'React' must be in scope when using JSX(react/react-in-jsx-scope)”**_ you need to add the next on the `eslintrc.json`:

```json
"rules": {
  // ...
  "react/react-in-jsx-scope": "off",
  "react/jsx-uses-react": "off",
  "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx", ".ts", ".tsx"] }],
}
```




# Backend

## Eslint

First we need to install the next packages:
```bash
yarn add -DE prettier eslint-plugin-prettier eslint-config-prettier eslint-import-resolver-typescript eslint-plugin-import
```
This command install the dependencies at the last exact version and as development dependencies.


Now we need to start the eslint configuration with the next command:
```bash
npx eslint --init
```

Now we need to follow the CLI to set up as we prefer the eslint configuration. After select the options this will generate a file for the configs (in my case I prefer to choose the `json` version), and we add the next:
```json
{
	...
	"settings": {
	    "import/resolver": {
		    "typescript": {}
	    }
	}
	...
}
```


## Prettier

Now we need to make the configurations to add prettier support to our default eslint rules. Now we need to install the next packages:

```bash
yarn add -DE prettier eslint-config-prettier eslint-plugin-prettier
```

And now we create a `.prettierrc` file with the next content:
```json
{
	"arrowParens": "avoid",
	"printWidth": 80,
	"semi": true,
	"singleQuote": true,
	"tabWidth": 2,
	"trailingComma": "all"
}
```


And now we make some changes on our eslint configurations file:
```json
{
	...
	"extends": [
		"eslint:recommended",
		"plugin:react/recommended",
		"plugin:@typescript-eslint/recommended",
		"prettier"
	],
	...
	"plugins": [
		"react",
		"@typescript-eslint",
		"prettier"
	],
	...
}
```

