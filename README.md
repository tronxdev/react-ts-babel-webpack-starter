## Create a new project with Typescript

> npx create-react-app my-app —-typescript

## Install ESLint packages

> npm i -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin

> npx eslint —init

> npm i -D eslint-config-standard eslint-plugin-import eslint-plugin-jest eslint-plugin-node eslint-plugin-promise eslint-plugin-react eslint-plugin-standard

## Put the followings in .eslintrc.json

```
{
  "env": {
    "browser": true,
    "es6": true
  },
  "parser": "@typescript-eslint/parser",
  "extends": [
    "standard",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:jest/recommended"
  ],
  "globals": {
    "Atomics": "readonly",
    "SharedArrayBuffer": "readonly"
  },
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 2018,
    "sourceType": "module"
  },
  "settings": {
    "react": {
      "version": "latest"
    }
  },
  "rules": {
    "@typescript-eslint/indent": "off",
    "@typescript-eslint/member-delimiter-style": "off",
    "@typescript-eslint/no-object-literal-type-assertion": "off",
    "jsx-quotes": ["error", "prefer-single"]
  }
}
```

> npx eslint src/\*.tsx

## Integrate Prettier with ESLint

> npm i -D prettier-eslint prettier-eslint-cli eslint-config-prettier eslint-plugin-prettier

## Put the following line in .eslintrc.json file

```
{
  "extends": ["plugin:prettier/recommended"]
}
```

## Create a custom VSCode settings

> mkdir .vscode

> touch .vscode/settings.json

## Add the following configuration to .vscode/settings.json file

```
{
  "editor.formatOnSave": true,
  "prettier.singleQuote": true
}
```

## Press CMD+P and type ‘Repload Window’ to enable the custom VSCode settings

## Create .eslintignore

> touch .eslintignore

## Configure .eslintignore

```
dist
node_modules
```

## Create .prettierrc file

> touch .prettierrc

## Configure Prettier in .prettierrc

```
{
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true,
  "jsxSingleQuote": true
}
```

## Update ‘scripts’ in package.json

```
"scripts": {
  "lint": "eslint '**/*.{js,ts,tsx}'",
  "lint:fix": "npm run lint -- --fix"
}
```

> npm run lint

> npm run lint:fix

## Install husky and lint-staged for pre-commit hooks

> npm i -D husky lint-staged

## Add the following lines to package.json

```
"husky": {
  "hooks": {
    "pre-commit": "lint-staged"
  }
},
"lint-staged": {
  "*.{js,ts,tsx}": [
    "npm run lint:fix",
    "git add"
  ]
},
```

## Create tsconfig.json file

> touch tsconfig.json

## Configure typescript in tsconfig.json file

```
{
  "compilerOptions": {
    "outDir": "./dist/",
    "target": "es5",
    // "lib": ["dom", "dom.iterable", "esnext"],
    // "allowJs": true,
    // "skipLibCheck": true,
    "esModuleInterop": true,
    // "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "moduleResolution": "node",
    // "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",
    "noImplicitAny": true,
    "sourceMap": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

## Create .babelrc file

> touch .babelrc

## Install @babel packages

> npm i -D babel-loader @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript

## Configure Babel in .babelrc file

```
{
  "presets": [
    "@babel/typescript",
    [
      "@babel/env",
      {
        "targets": "node 10"
      }
    ],
    "@babel/react"
  ],
  "env": {
    "esm": {
      "presets": [
        "@babel/typescript",
        [
          "@babel/env",
          {
            "modules": false
          }
        ],
        "@babel/react"
      ]
    }
  }
}
```

## Install Webpack packages and loaders

> npm i -D webpack webpack-cli webpack-dev-server html-webpack-plugin css-loader file-loader html-loader style-loader ts-loader

## Create webpack.config.js file

> touch webpack.config.js

## Configure Webpack with Babel in webpack.config.js file

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.tsx',
  output: {
    path: path.join(__dirname, '/dist'),
    filename: 'bundle.js'
  },
  devtool: 'inline-source-map',
  module: {
    rules: [
      {
        test: /\.(ts|js)x?$/,
        exclude: /node_modules/,
        use: 'babel-loader'
      },
      {
        test: /\.ts?$/,
        use: 'ts-loader',
        exclude: /node_modules/
      },
      {
        test: /\.html$/,
        use: 'html-loader'
      },
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader']
      },
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: ['file-loader']
      }
    ]
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js']
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, '/src/index.html'),
      filename: './index.html'
    })
  ],
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    compress: true,
    port: 3000
  }
};
```

## Remove “‘main’: ‘index.js’” and add “‘private’: true” in package.json file

```
// "main": "index.js",
"private": true,
```

## Add npm scripts for webpack shortcut in package.json file

```
"scripts": {
  "start": "webpack-dev-server --mode development --open",
  "build": "webpack --mode production",
}
```

## Test the scripts in terminal

> npm start

> npm run build
