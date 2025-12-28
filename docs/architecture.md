# Architecture

High-level components:

1) Extension host (Node context)
- Entry: src/extension.ts – registers commands, providers, webview panels, and sidebar provider.
- Providers: src/providers – LLMProvider (abstract) with implementations:
  - ClaudeApiProvider – talks to Anthropic via SDK/API
  - ClaudeCodeProvider – shells out to "claude"/"claude-code" binary
  - LLMProviderFactory – lifecycle, selection, validation, status
- Services: src/services – chatMessageService, customAgentService, claudeCodeService, logger
- Tools: src/tools – workspace operations exposed to the agent (read, write, edit, ls, grep, glob, multiedit, theme, bash)
- Templates: src/templates/webviewTemplate.ts – builds HTML and injects context for webviews

2) Webview (React, browser context)
- Entry: src/webview/index.tsx – decides between chat/panel and canvas view based on data-view attribute
- App: src/webview/App.tsx – renders ChatInterface or CanvasView; injects CSS; acquires VS Code API
- Components:
  - Canvas: CanvasView, ConnectionLines, DesignFrame, DesignPanel
  - Chat: ChatInterface, ModelSelector, ModeToggle, Theme* components
  - Welcome: initial screen components
- Utilities: themeParser, gridLayout
- Types: canvas.types.ts, webview types

Messaging and context:
- webviewTemplate.ts serializes a WebviewContext into window.__WEBVIEW_CONTEXT__ for the chat view.
- For canvas view, App reads data-view='canvas' and CanvasView requests data directly from extension via message passing.

Security/CSP:
- CSP: default-src 'none'; style-src webview.cspSource 'unsafe-inline'; img-src webview.cspSource data: https: vscode-webview:; script-src webview.cspSource 'unsafe-inline'; font-src webview.cspSource
- Nonce handling: App reads a data-nonce attribute when present; template provides a single bundled webview.js script.

Logging:
- Logger service writes to a "Superdesign" output channel and can surface notifications for INFO/WARN/ERROR.
