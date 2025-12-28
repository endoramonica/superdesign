# Setup

Prerequisites:
- VS Code >= 1.90 (per engines.vscode in package.json)
- Node.js 18+ recommended
- pnpm or npm

Install dependencies:
- npm install
  or
- pnpm install

Build:
- Development build: npm run compile (type-check, lint, esbuild)
- Watch mode: npm run watch (tsc and esbuild watchers in parallel)
- Production package: npm run package (type-check, lint, esbuild --production)

Run the extension:
- Press F5 in VS Code to launch Extension Development Host, or
- Use @vscode/test-electron via npm run test (see scripts)

Testing:
- Unit/integration tests are invoked using custom test runners compiled to dist-test.
- Available subsets (require tsconfig.test.json):
  - npm run test:llm
  - npm run test:core
  - npm run test:read
  - npm run test:write-edit
  - npm run test:ls-grep-glob
  - npm run test:agent
  - npm run test:tools

Publishing:
- The repo includes .github/workflows/publish.yml with a basic publish flow.

Assets and bundling:
- esbuild.js drives bundling of extension code and webview React app outputs to dist/.
- Webview entry is src/webview/index.tsx; template references dist/webview.js.
