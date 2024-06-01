# Some package related knowledge

## 1. Module resolution
It's crutial to understand how node, typescript compiler or other tools do module resolution.

### 1.1 Node js module resolution
[Jump to Make your Typescript monorepo live](#2-make-your-typescript-monorepo-live)


1. Core Modules
   - **First Check**: Node.js checks if the module is a core module (e.g., `fs`, `path`).
   - **Examples**:
     ```javascript
     import fs from 'fs';
     import path from 'path';
     ```

2. Relative Modules
   - **Second Check**: Node.js checks if the import starts with `/`, `./`, or `../`.
   - **Path Resolution**:
     - If the path starts with `/`, it looks from the root of the file system.
     - If the path starts with `./` or `../`, it looks relative to the current file's location.
   - **File Extensions**:
     - If `foo` exists, but without an extension, it throws an `ERR_UNKNOWN_FILE_EXTENSION`.
     - Checks for `foo.js`, `foo.json`, `foo.node` (binary add-on), `foo/package.json` (looking for the `main` field), and `foo/index.js`.

3. Alias Modules
   - **Third Check**: Node.js checks if the import starts with `#` (alias).
   - **Alias Resolution**:
     - Scans for the nearest `package.json`.
     - Searches for the `imports` field in `package.json`.
     - Resolves the alias to the configured path.

4. Self-Import Modules
   - **Fourth Check**: Node.js checks if the import starts with the package name of the current scope.
   - **Self-Import Resolution**:
     - Similar to the `imports` field resolution but requires the import to start with the package name.
     - Can be used to import internal modules from anywhere in the project.

5. Node Modules
   - **Final Check**: If none of the previous checks pass, Node.js checks the `node_modules` directories.
   - **Node Modules Resolution**:
     - Checks each parent directory up to the root for a `node_modules` directory.
     - Looks for node_modules/foo/package.json and resolves based on the following fields (in order):
        - `exports` field: Resolves the module according to the exports field configuration.
        - `main` field: Uses the main field to resolve the entry point if exports is not found.
        - If neither exports nor main fields are found, it falls back to:
          - `node_modules/foo/index.js`
          - `node_modules/foo/index.json`
          - `node_modules/foo/index.node`
## 2. Make your Typescript monorepo live
Best strategie to make TypeScript monorepos feel "alive" during development. The goal is to have TypeScript code changes propagate instantly across the monorepo without needing a build/compile step. 

### Use publishConfig
Overview: Ensures TypeScript and Node.js resolve package imports to local .ts files during development, and to compiled .js files in production.

```json
{
  "name": "pkg-a",
  "main": "./src/index.ts",
  "types": "./src/index.ts",
  "exports": {
    ".": {
      "import": "./src/index.ts",
      "require": "./src/index.ts",
      "types": "./src/index.ts"
    },
    "./package.json": "./package.json"
  },
  "publishConfig": {
    "main": "./lib/index.js",
    "types": "./lib/index.d.ts",
    "exports": {
      ".": {
        "import": "./lib/index.js",
        "require": "./lib/index.js",
        "types": "./lib/index.d.ts"
      },
      "./package.json": "./package.json"
    }
  }
}
```





