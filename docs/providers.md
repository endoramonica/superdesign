# LLM Providers

Abstractions:
- LLMProvider (abstract): initialize, query, readiness, configuration validation, and provider metadata.
- LLMProviderFactory: singleton managing provider instances, configuration, switching, validation, and status.

Implementations:
- ClaudeApiProvider (type: api)
  - Uses Anthropic SDK/API with superdesign.anthropicApiKey and optional superdesign.anthropicUrl
  - Exposes streaming via onMessage callback
  - isAuthError() signals credential issues
- ClaudeCodeProvider (type: binary)
  - Invokes local claude/claude-code binary (configurable via superdesign.claudeCodePath)
  - Supports model selection (superdesign.claudeCodeModelId) and thinking budget
  - Provides detailed error messages if binary not found or misconfigured

Selecting providers:
- Configuration key: superdesign.llmProvider ('claude-api' | 'claude-code')
- Programmatic switch: LLMProviderFactory.switchProvider()
- Status for UI: LLMProviderFactory.getProviderStatus()

Custom agent settings (optional):
- superdesign.aiModelProvider, superdesign.aiModel, superdesign.openaiApiKey, superdesign.openaiUrl, superdesign.openrouterApiKey
- Note: For Claude Code, prefer using the main provider path, not the custom agent setting.
