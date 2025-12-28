# Configuration

Settings (contributes.configuration):

- superdesign.llmProvider (string; default: "claude-api")
  - Values: "claude-api", "claude-code"
  - Chooses the main LLM provider used by the extension.

- superdesign.anthropicApiKey (string)
  - Anthropic API key for the Claude API provider.

- superdesign.anthropicUrl (string)
  - Optional custom Anthropic API URL (e.g., local proxy like http://localhost:4000).

- superdesign.claudeCodePath (string)
  - Path to the Claude Code binary. Defaults to "claude" if installed via npm; use "claude-code" if installed via shell script.

- superdesign.claudeCodeModelId (string; default: "claude-sonnet-4-20250514")
  - Enum options include:
    - claude-sonnet-4-20250514
    - claude-3-5-sonnet-20241022
    - claude-3-5-haiku-20241022
    - claude-3-opus-20240229

- superdesign.claudeCodeThinkingBudget (number; default: 50000)
  - Thinking budget tokens for the Claude Code provider.

- superdesign.openaiApiKey (string)
- superdesign.openaiUrl (string)
- superdesign.openrouterApiKey (string)
- superdesign.aiModelProvider (string; default: "anthropic")
  - Enum: openai | anthropic | openrouter | claude-code
  - Note: For Claude Code, prefer to use the main LLM Provider setting above.
- superdesign.aiModel (string)
  - Specific model identifier (e.g., gpt-4o, claude-3-5-sonnet-20241022)

Quick commands to configure:
- Superdesign: Configure Anthropic API Key
- Superdesign: Configure OpenAI API Key
- Superdesign: Configure OpenRouter API Key
- Superdesign: Configure OpenAI url
- Superdesign: Open Settings (filters to extension settings)

Provider selection:
- The active provider is read from superdesign.llmProvider via LLMProviderFactory.getProvider().
- You can switch providers programmatically via LLMProviderFactory.switchProvider().
