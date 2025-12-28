# Built-in Tools

Located in src/tools/, these tools power workspace interactions used by the agent and chat services.

- read-tool.ts – read files from the workspace with filtering
- write-tool.ts – write/append files safely
- edit-tool.ts – structured edits to file regions
- multiedit-tool.ts – batch edits across multiple files
- ls-tool.ts – list files, with ignore patterns
- grep-tool.ts – regex search across files
- glob-tool.ts – globbing utilities
- theme-tool.ts – theme inspection/selection helpers
- bash-tool.ts – execute shell commands safely with timeouts and capture
- tool-utils.ts – common helpers and guards

Security considerations:
- bash-tool checks for potentially unsafe commands and enforces timeouts
- write/edit tools should be constrained to the current workspace folder
