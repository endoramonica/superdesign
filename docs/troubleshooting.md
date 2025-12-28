# Troubleshooting

No UI appears in sidebar:
- Ensure the view container 'Superdesign' is visible in the Activity Bar
- Run command: Superdesign: Show Chat Sidebar

Canvas view shows blank:
- Use Superdesign: Open Canvas View
- Check the DevTools console (Help > Toggle Developer Tools) in the webview for errors

Claude API auth errors:
- Configure superdesign.anthropicApiKey
- If using a proxy, set superdesign.anthropicUrl
- LLMProviderFactory.getProviderStatus() can help diagnose

Claude Code not found:
- Ensure the binary is installed and accessible (claude or claude-code)
- Set superdesign.claudeCodePath and restart
- Verify model id in superdesign.claudeCodeModelId

Script injection fails in iframe:
- Use srcdoc or ensure same-origin content
- Wrap injection on iframe load event; handle exceptions
- Forward messages using VS Code webview postMessage APIs

Build failures:
- Run npm run check-types and npm run lint for specifics
- Delete node_modules and reinstall if necessary
