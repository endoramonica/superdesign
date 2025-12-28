# Overview

SuperDesign is an open-source design agent extension for VS Code-family IDEs (VS Code, Cursor, Windsurf, Claude Code). It generates UI mockups, components, and wireframes from natural language prompts and provides a canvas view to preview and iterate on designs.

Key features:
- Product Mock: Generate full UI screens from a prompt
- UI Components: Create reusable components to paste into projects
- Wireframes: Rapid low-fidelity exploration
- Fork & Iterate: Duplicate designs and evolve options
- Prompt-to-IDE: Works with Cursor, Windsurf, Claude Code, and vanilla VS Code
- Storage: Designs saved locally under .superdesign/

Top-level structure:
- src/extension.ts – main extension entrypoint and registration
- src/providers/ – LLM provider abstraction and implementations
- src/services/ – shared services (chat, custom agent, logger)
- src/tools/ – workspace tool implementations (bash, read, write, grep, glob, etc.)
- src/templates/webviewTemplate.ts – HTML template for webviews (chat panel and canvas)
- src/webview/ – React UI for chat and canvas (bundled via esbuild)

Relevant docs:
- ./commands.md – how to open chat sidebar and canvas
- ./providers.md – selecting LLM provider (Claude API vs Claude Code)
- ./webview.md – webview architecture and CSP
