# Security

Permissions and scope:
- The extension operates within the active VS Code workspace. File tools (read/write/edit/grep/glob) are scoped to the workspace folder.

CSP in webviews:
- default-src 'none'; script-src and style-src limited to webview.cspSource; images may use data:, https:, vscode-webview: origins.
- A single bundled script (dist/webview.js) is loaded. Avoid adding additional script tags or remote scripts.

Secrets:
- API keys are stored in VS Code settings (application scope). Do not hardcode keys in source.
- Logger can surface messages; do not log secrets.

Shell execution safety:
- bash-tool includes command checks and timeouts to mitigate risk.

Cross-frame messaging:
- Validate and scope postMessage handlers where possible; avoid wildcard '*' in production and consider origin checks if you control the iframe content.
