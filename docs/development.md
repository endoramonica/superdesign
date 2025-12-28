# Development

Scripts (package.json):
- compile: type-check, lint, esbuild bundle
- watch: watch:esbuild + watch:tsc in parallel
- package: production build
- test: vscode-test (see also test:*)
- lint: eslint src

Type checking:
- tsconfig.json for extension code
- tsconfig.test.json for test compilation to dist-test

Linting:
- eslint.config.mjs covers TypeScript rules for src/

Building webviews:
- esbuild.js bundles the React code (src/webview/*) into dist/webview.js
- App.css is imported as a string and injected at runtime

Debugging:
- Use VS Code launch for Extension Development Host
- View 'Superdesign' output channel for logs (Logger)

Project initialization:
- Command: Superdesign: Initialize Superdesign – sets up basic project scaffolding and rules for supported IDEs
