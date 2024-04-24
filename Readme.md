# Setup

## To initialize the project with git along with React and Typescript

```
git init
npm init -y
npm install -D react @types/react typescript

```

we need to move react to peerDependencies because it’s typically used as a peer dependency in library packages. This allows consumers to use one version of React without conflicts. To do this, add the following lines to your package.json and remove React from devDependencies:

"peerDependencies": {
"react": "^18.2.0"
},
`

## To install Prettier for formatting, run the following command:

`
npm install -D prettier

Create`.prettierrc` at the root of the project and set rules as follows:

```{
"printWidth": 80,
"tabWidth": 2
}
```

# ESLint

Use ESLint to checks your JavaScript code for common problems, such as syntax errors, formatting issues, code style violations, and potential bugs.

To install the ESLint with its plugin, run the following command:

`npm install -D eslint @typescript-eslint/parser eslint-config-prettier eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-storybook @typescript-eslint/eslint-plugin`

Create a config file for ESLint named `.eslintrc` at the root of the project and paste the following configuration:

```js
   {
 // Specify the environments where the code will run
 "env": {
   "jest": true,       // Enable Jest for testing
   "browser": true     // Enable browser environment
 },

 // Stop ESLint from searching for configuration in parent folders
 "root": true,

 // Specify the parser for TypeScript (using @typescript-eslint/parser)
 "parser": "@typescript-eslint/parser", // Leverages TS ESTree to lint TypeScript

 // Add additional rules and configuration options
 "plugins": ["@typescript-eslint"],

 // Extend various ESLint configurations and plugins
 "extends": [
   "eslint:recommended",                   // ESLint recommended rules
   "plugin:react/recommended",             // React recommended rules
   "plugin:@typescript-eslint/recommended", // TypeScript recommended rules
   "plugin:@typescript-eslint/eslint-recommended", // ESLint overrides for TypeScript
   "prettier",                             // Prettier rules
   "plugin:prettier/recommended",          // Prettier plugin integration
   "plugin:react-hooks/recommended",       // Recommended rules for React hooks
   "plugin:storybook/recommended"          // Recommended rules for Storybook
 ],
 "rules": {
   "react/react-in-jsx-scope": "off",
 }
}
```

For linting the project, add the following script in package.json

```{
  ...
  "scripts": {
    "lint": "eslint . --ext .ts,.tsx --ignore-path .gitignore --fix"
  },
  ...
}
```

# GitIgnore

Create a `.gitignore` file in the root directory and add directories that we don’t have to include in the repository.

```

node_modules
dist
#storybook build directory
storybook-static

```

# Typescript and Vite Configuration

Create a file named `tsconfig.json `and paste the following configuration:

```
{
  "compilerOptions": {
    "target": "ES5", // Specifies the JavaScript version to target when transpiling code.
    "useDefineForClassFields": true, // Enables the use of 'define' for class fields.
    "lib": ["ES2020", "DOM", "DOM.Iterable"], // Specifies the libraries available for the code.
    "module": "ESNext", // Defines the module system to use for code generation.
    "skipLibCheck": true, // Skips type checking of declaration files.

    /* Bundler mode */
    "moduleResolution": "bundler", // Specifies how modules are resolved when bundling.
    "allowImportingTsExtensions": true, // Allows importing TypeScript files with extensions.
    "resolveJsonModule": true, // Enables importing JSON modules.
    "isolatedModules": true, // Ensures each file is treated as a separate module.
    "noEmit": true, // Prevents TypeScript from emitting output files.
    "jsx": "react-jsx", // Configures JSX support for React.

    /* Linting */
    "strict": true, // Enables strict type checking.
    "noUnusedLocals": true, // Flags unused local variables.
    "noUnusedParameters": true, // Flags unused function parameters.
    "noFallthroughCasesInSwitch": true, // Requires handling all cases in a switch statement.
    "declaration": true, // Generates declaration files for TypeScript.
  },
  "include": ["src"], // Specifies the directory to include when searching for TypeScript files.
  "exclude": [
    "src/**/__docs__","src/**/__test__"
  ]
}
```

Install Vite with one plugin that generates a declaration file
`npm install -D vite vite-plugin-dts`

Create a file named `vite.config.ts` in the root directory with the following configuration:

```

import { defineConfig } from "vite";
import dts from "vite-plugin-dts";
import { peerDependencies } from "./package.json";

export default defineConfig({
  build: {
    lib: {
      entry: "./src/index.ts", // Specifies the entry point for building the library.
      name: "vite-react-ts-button", // Sets the name of the generated library.
      fileName: (format) => `index.${format}.js`, // Generates the output file name based on the format.
      formats: ["cjs", "es"], // Specifies the output formats (CommonJS and ES modules).
    },
    rollupOptions: {
      external: [...Object.keys(peerDependencies)], // Defines external dependencies for Rollup bundling.
    },
    sourcemap: true, // Generates source maps for debugging.
    emptyOutDir: true, // Clears the output directory before building.
  },
  plugins: [dts()], // Uses the 'vite-plugin-dts' plugin for generating TypeScript declaration files (d.ts).
});
```

Add this configuration to package.json

```
{
  ...
  "type": "module",
  "main": "dist/index.cjs.js",
  "module": "dist/index.es.js",
  "types": "dist/index.d.ts",
  "files": [
    "/dist"
  ],
  "scripts":{
   ...
   "build": "tsc && vite build",
  }
}
```

# Adding your Styles Library or Framework

Any library/Framework, can work

# Creating Components

- Create a `src/components` folder in the root directory
- Create a folder named button for our button components. Add `Button.tsx` and `index.ts` in this folder
  in index.ts export the Button compoenet export { default as Button } from './Button';

Add an index.ts file to the components folder, as this file will allow you to export all the components from the components folder.
example export \* from './button'; // Add more exports for other components as needed

Add an index.ts file to the src folder as it serves as the entry point for your entire library.
example // `src/index.ts`
export \* from './components'; // This will export all components from the 'components' folder

# Run build

After adding a component, run the following command. It generates a ‘dist’ folder.

`npm run build`

# Adding Storybook and Husky

## Storybook

`npx storybook@latest init`
