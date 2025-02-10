# Setup Plans

1. Basic project setup
2. ESLint & Prettier setup
3. Husky and Lint Stage

### Basic project setup

```tsx
// Installation
// Commands from the terminal

yarn init -y
yarn add -D typescript
tsc --init

// paste and replace "rootDir": "./src", "outDir": "./dist" into tsconfig.json file

yarn add cors dotenv express mongoose
yarn add -D ts-node-dev @types/express @types/cors
```

```json
// package.json
{
  "name": "ums-auth-service",
  "version": "1.0.0",
  "main": "src/server.js",
  "license": "MIT",
  "scripts": {
    "start": "ts-node-dev --respawn --transpile-only src/server.ts",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Independent University Bangladesh",
  "devDependencies": {
    "@types/cors": "^2.8.13",
    "@types/express": "^4.17.17",
    "ts-node-dev": "^2.0.0",
    "typescript": "^5.0.4"
  },
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "mongoose": "^7.2.1"
  }
}
```

```
node_modules
.env
```

```
NODE_ENV=development
PORT=5000
DATABASE_URL=mongodb+srv://warehouse:3Fqyo8QaaI3PAcfk@cluster0.vzdnu.mongodb.net/IUB?retryWrites=true&w=majority
```

```tsx
// src/config/index.ts
import dotenv from "dotenv";
import path from "path";
dotenv.config({ path: path.join(process.cwd(), ".env") });

export default {
  port: process.env.PORT,
  database_url: process.env.DATABASE_URL,
};
```

```tsx
// src/server.ts
import mongoose from "mongoose";
import app from "./app";
import config from "./config";

async function bootstrap() {
  try {
    await mongoose.connect(config.database_url as string);
    console.log("Database is connected successfully");

    app.listen(config.port, () => {
      console.log(`Application listening on port ${config.port}`);
    });
  } catch (error) {
    console.log("Failed to connect database", error);
  }
}

bootstrap();
```

```tsx
// src/app.ts
import express, { Application, Request, Response } from "express";
import cors from "cors";
const app: Application = express();

app.use(cors());

// Parser
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// testing
app.get("/", (req: Request, res: Response) => {
  res.send("Working!");
});

export default app;
```

### ESLint & Prettier setup

https://blog.logrocket.com/linting-typescript-eslint-prettier/

Install “eslint” and “prettier” extensions on vs code editor if not installed

Add more two fields to tsconfig.json file "include", "exclude"

```
// tsconfig.json
{
  "compilerOptions": {
    "outDir": "dist", // where to put the compiled JS files
    "target": "ES2020", // which level of JS support to target
    "module": "CommonJS", // which system for the program AMD, UMD, System, CommonJS

    // Recommended: Compiler complains about expressions implicitly typed as 'any'
    "noImplicitAny": true,
  },
  "include": ["src"], // which files to compile
  "exclude": ["node_modules"], // which files to skip
}
```

```tsx
// Installation

yarn add eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev
yarn add -D prettier
```

```json
// .eslintrc
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  // HERE
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],

  "rules": {
    // to enforce following these rules or these rules will throw an error
    "no-unused-vars": "error",
    "no-console": "error",
    "no-undef": "error",
    "no-unused-expressions": "error",
    "no-unreachable": "error",
    "@typescript-eslint/consistent-type-definitions": ["error", "type"]
  },

  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "globals": {
    "process": "readonly"
  }
}
```

```json
// package.json
{
  "name": "ums-auth-service",
  "version": "1.0.0",
  "main": "src/server.js",
  "license": "MIT",
  "scripts": {
    "start": "ts-node-dev --respawn --transpile-only src/server.ts",
    "lint:check": "eslint --ignore-path .eslintignore --ext .js,.ts .",
    "lint:fix": "eslint . --fix",
    "prettier:check": "prettier --ignore-path .gitignore --write \"**/*.+(js|ts|json)\"",
    "prettier-fix": "prettier --write .",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Independent University Bangladesh",
  "devDependencies": {
    "@types/cors": "^2.8.13",
    "@types/express": "^4.17.17",
    "ts-node-dev": "^2.0.0",
    "typescript": "^5.0.4"
  },
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "mongoose": "^7.2.1"
  }
}
```

```
node_modules
dist
.env
```

```json
// .prettierrc
{
  "semi": true, // Specify if you want to print semicolons at the end of statements
  "singleQuote": true, // If you want to use single quotes
  "arrowParens": "avoid" // Include parenthesis around a sole arrow function parameter
}
```

```json
// settings.json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true
}
```

```json
yarn add -D eslint-config-prettier

// .eslintrc
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  // HERE
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],

  "rules": {
    // to enforce following these rules or these rules will throw an error
    "no-unused-vars": "error",
    "no-console": "error",
    "no-undef": "error",
    "no-unused-expressions": "error",
    "no-unreachable": "error",
    "@typescript-eslint/consistent-type-definitions": ["error", "type"]
  },

  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "globals": {
    "process": "readonly"
  }
}
```

### Husky and Lint Stage

```tsx
// Installation

yarn add -D husky
yarn add -D lint-staged
yarn husky install

// After installing husky, a folder “.husky” will be created. Then run another command -

yarn husky add .husky/pre-commit "yarn lint-staged"

// It will create another file "pre-commit" -
// ...
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

yarn lint-staged
// ...

// After that create another script inside the "script" property in the "package.json" file
"lint-prettier": "yarn lint:check && yarn prettier:check"

// After that create another property inside the "package.json" file
"lint-staged": {
	"src/**/*.ts": "yarn lint-prettier"
}

// Now by every commit, it will run lint:check script to check eslint issues
```
