# 📊 Superdesign - Codebase Summary

## 🎯 Tổng Quan

**Superdesign** là một VS Code Extension tích hợp AI Agent cho phép người dùng thiết kế UI/Frontend trực tiếp trong IDE. Ứng dụng sử dụng Claude AI (hoặc các LLM khác) để tạo, lặp lại và quản lý các thiết kế HTML/SVG.

---

## 📈 Thống Kê Codebase

### Cấu Trúc Thư Mục
```
Total Directories: 15
Total Files: 50+
Main Language: TypeScript/React
```

### Phân Bố File

| Thư Mục | Số File | Mục Đích |
|---------|---------|---------|
| `src/` | 1 | Entry point |
| `src/assets/` | 7 | Logo & icons |
| `src/providers/` | 5 | VS Code providers |
| `src/services/` | 4 | Business logic |
| `src/tools/` | 10 | AI agent tools |
| `src/types/` | 2 | Type definitions |
| `src/templates/` | 1 | HTML templates |
| `src/webview/` | 1 | React entry |
| `src/webview/components/` | 8 | React components |
| `src/webview/hooks/` | 2 | React hooks |
| `src/webview/types/` | 1 | Canvas types |
| `src/webview/utils/` | 2 | Utilities |

---

## 🏗️ Kiến Trúc Tổng Thể

### Layer 1: Extension Layer (Backend)
```
extension.ts
├── Command Registration
├── Provider Setup
├── Message Handling
└── File Operations
```

### Layer 2: Service Layer
```
Services/
├── CustomAgentService (AI orchestration)
├── ChatMessageService (Message handling)
├── ClaudeCodeService (Claude Code integration)
└── Logger (Logging)
```

### Layer 3: Tool Layer
```
Tools/
├── File Operations (read, write, edit, multiedit)
├── System Operations (bash, glob, grep, ls)
├── Theme Generation (theme-tool)
└── Utilities (tool-utils)
```

### Layer 4: Provider Layer
```
Providers/
├── LLM Provider Factory
├── Anthropic Provider
├── OpenAI Provider
├── OpenRouter Provider
└── Claude Code Provider
```

### Layer 5: Webview Layer (Frontend)
```
Webview/
├── App (Root component)
├── Chat Interface
├── Canvas View
├── Components (Design Frame, Connection Lines, etc.)
├── Hooks (useChat, useFirstTimeUser)
└── Utils (gridLayout, themeParser)
```

---

## 🎯 Chức Năng Chính (20 Features)

### 1️⃣ Chat Interface
- **File:** `src/webview/components/Chat/ChatInterface.tsx`
- **Chức năng:** Giao diện chat tương tác với AI
- **Tính năng:** Message history, markdown rendering, streaming responses

### 2️⃣ Canvas View
- **File:** `src/webview/components/CanvasView.tsx`
- **Chức năng:** Hiển thị grid các design files
- **Tính năng:** Zoom, pan, frame selection, file watching

### 3️⃣ Design Frame Component
- **File:** `src/webview/components/DesignFrame.tsx`
- **Chức năng:** Render HTML/SVG trong iframe
- **Tính năng:** Viewport switching, floating buttons, drag & drop

### 4️⃣ AI Agent Service
- **File:** `src/services/customAgentService.ts`
- **Chức năng:** Orchestrate AI requests & tool execution
- **Tính năng:** Multi-provider support, streaming, error handling

### 5️⃣ LLM Provider Factory
- **File:** `src/providers/llmProviderFactory.ts`
- **Chức năng:** Create & manage LLM providers
- **Tính năng:** Anthropic, OpenAI, OpenRouter, Claude Code

### 6️⃣ Read Tool
- **File:** `src/tools/read-tool.ts`
- **Chức năng:** Đọc file contents
- **Tính năng:** Line range support, multiple files

### 7️⃣ Write Tool
- **File:** `src/tools/write-tool.ts`
- **Chức năng:** Tạo/ghi file mới
- **Tính năng:** Auto directory creation, multiple files

### 8️⃣ Edit Tool
- **File:** `src/tools/edit-tool.ts`
- **Chức năng:** Thay thế text trong file
- **Tính năng:** Exact matching, preserve formatting

### 9️⃣ Bash Tool
- **File:** `src/tools/bash-tool.ts`
- **Chức năng:** Thực thi shell commands
- **Tính năng:** Timeout, output capture, error handling

### 🔟 Glob Tool
- **File:** `src/tools/glob-tool.ts`
- **Chức năng:** Tìm files matching patterns
- **Tính năng:** Efficient file discovery

### 1️⃣1️⃣ Grep Tool
- **File:** `src/tools/grep-tool.ts`
- **Chức năng:** Tìm text patterns
- **Tính năng:** Regex support, context lines

### 1️⃣2️⃣ Theme Tool
- **File:** `src/tools/theme-tool.ts`
- **Chức năng:** Tạo CSS theme files
- **Tính năng:** OKLCH color system, CSS variables

### 1️⃣3️⃣ Viewport Control
- **File:** `src/webview/components/DesignFrame.tsx`
- **Chức năng:** Switch Mobile/Tablet/Desktop views
- **Tính năng:** Per-frame & global viewport control

### 1️⃣4️⃣ Copy Prompt Feature
- **File:** `src/webview/components/DesignFrame.tsx`
- **Chức năng:** Copy design cho AI platforms
- **Tính năng:** Cursor, Windsurf, Claude Code, Lovable, Bolt

### 1️⃣5️⃣ Image Management
- **File:** `src/extension.ts`
- **Chức năng:** Lưu images vào moodboard
- **Tính năng:** Base64 conversion, MIME detection

### 1️⃣6️⃣ Project Initialization
- **File:** `src/extension.ts`
- **Chức năng:** Tạo `.superdesign/` structure
- **Tính năng:** Auto folder creation, design rules setup

### 1️⃣7️⃣ Connection Lines
- **File:** `src/webview/components/ConnectionLines.tsx`
- **Chức năng:** Vẽ đường kết nối giữa frames
- **Tính năng:** SVG rendering, hierarchy visualization

### 1️⃣8️⃣ Markdown Renderer
- **File:** `src/webview/components/MarkdownRenderer.tsx`
- **Chức năng:** Render markdown với syntax highlighting
- **Tính năng:** Code blocks, tables, links

### 1️⃣9️⃣ Grid Layout System
- **File:** `src/webview/utils/gridLayout.ts`
- **Chức năng:** Auto-layout frames trên canvas
- **Tính năng:** Position calculation, collision detection

### 2️⃣0️⃣ Logger Service
- **File:** `src/services/logger.ts`
- **Chức năng:** Centralized logging
- **Tính năng:** Multiple log levels, output channel

---

## 🔄 Data Flow

### Chat Message Flow
```
User Input
  ↓
ChatMessageService.handleChatMessage()
  ↓
LLMProviderFactory.getModel()
  ↓
streamText() with tools
  ↓
Tool Execution (read, write, bash, etc.)
  ↓
Response Stream
  ↓
Webview Display
```

### Design File Creation Flow
```
User Request
  ↓
AI Agent Analysis
  ↓
Write Tool Execution
  ↓
File System Write
  ↓
File Watcher Detection
  ↓
Canvas Update
  ↓
Frame Display
```

---

## 🛠️ Technology Stack

### Backend
- **Language:** TypeScript
- **Runtime:** Node.js
- **Framework:** VS Code Extension API
- **AI SDK:** Vercel AI SDK
- **LLM Providers:** Anthropic, OpenAI, OpenRouter, Claude Code

### Frontend
- **Framework:** React 19
- **Language:** TypeScript
- **Styling:** CSS + CSS Variables
- **Libraries:** 
  - react-markdown (Markdown rendering)
  - react-zoom-pan-pinch (Canvas zoom/pan)
  - lucide-react (Icons)
  - highlight.js (Syntax highlighting)

### Build Tools
- **Bundler:** esbuild
- **Linter:** ESLint
- **Type Checker:** TypeScript
- **Package Manager:** npm/pnpm

---

## 📦 Dependencies (Key)

### Production
```json
{
  "vscode": "^1.90.0",
  "ai": "^4.3.16",
  "@ai-sdk/anthropic": "^1.2.12",
  "@ai-sdk/openai": "^1.3.22",
  "@openrouter/ai-sdk-provider": "^0.7.2",
  "@anthropic-ai/claude-code": "^1.0.31",
  "react": "^19.1.0",
  "react-dom": "^19.1.0",
  "react-markdown": "^10.1.0",
  "react-zoom-pan-pinch": "^3.7.0",
  "highlight.js": "^11.11.1",
  "lucide-react": "^0.522.0",
  "glob": "^11.0.3",
  "micromatch": "^4.0.8",
  "mime-types": "^3.0.1",
  "zod": "^3.25.67"
}
```

### Development
```json
{
  "typescript": "^5.8.3",
  "eslint": "^9.25.1",
  "@typescript-eslint/eslint-plugin": "^8.31.1",
  "esbuild": "^0.25.3",
  "@vscode/test-electron": "^2.5.2"
}
```

---

## 🎯 Key Concepts

### 1. **Workspace-Scoped Operations**
- Tất cả file operations được scoped vào workspace root
- `.superdesign/` folder là root cho design files
- Moodboard images được lưu trong `.superdesign/moodboard/`

### 2. **Design File Hierarchy**
- Parent-child relationships giữa design files
- Versioning thông qua naming convention: `design_name_n.html`
- Connection lines visualize hierarchy

### 3. **Multi-Provider Support**
- Flexible LLM provider selection
- Fallback mechanisms
- Configuration-driven provider selection

### 4. **Streaming Responses**
- Real-time AI responses
- Tool execution during streaming
- Progressive UI updates

### 5. **Security-First Design**
- CSP headers for iframes
- Nonce injection for scripts
- Service Worker for external resources
- API key encryption

### 6. **Responsive Canvas**
- Zoom-based lazy loading
- Efficient grid layout
- Pan & zoom controls
- Frame selection & interaction

---

## 📊 File Statistics

### Largest Files
1. `src/extension.ts` - ~1932 lines (Main extension logic)
2. `src/webview/components/DesignFrame.tsx` - ~838 lines (Design frame component)
3. `src/services/customAgentService.ts` - ~500+ lines (AI agent service)

### Most Complex Components
1. **DesignFrame.tsx** - Iframe rendering, viewport control, floating buttons
2. **CanvasView.tsx** - Grid layout, zoom/pan, file watching
3. **CustomAgentService.ts** - Multi-provider support, tool execution

---

## 🚀 Performance Characteristics

### Optimization Strategies
- ✅ Lazy loading iframes (zoom-based)
- ✅ Service Worker for external resources
- ✅ Efficient grid layout calculation
- ✅ Streaming responses for large outputs
- ✅ Debounced zoom/pan events
- ✅ Memoized components

### Scalability
- Supports 100+ design files
- Efficient file watching
- Streaming for large responses
- Lazy loading for performance

---

## 🔐 Security Measures

1. **API Key Management**
   - Stored in VS Code secure storage
   - Never exposed in logs
   - Encrypted at rest

2. **Iframe Security**
   - Content Security Policy headers
   - Nonce injection for scripts
   - Restricted resource loading

3. **Message Validation**
   - Type checking
   - Command validation
   - Payload sanitization

4. **File System Access**
   - Workspace-scoped operations
   - Path validation
   - Permission checks

5. **External Resources**
   - Service Worker for CORS
   - Fallback image generation
   - Error handling

---

## 🎨 UI/UX Features

- ✅ Dark mode support
- ✅ Responsive design
- ✅ Smooth animations
- ✅ Drag & drop
- ✅ Floating action buttons
- ✅ Connection visualization
- ✅ Loading states
- ✅ Error handling
- ✅ Viewport switching
- ✅ Zoom & pan controls

---

## 📝 Code Quality

### Practices
- TypeScript for type safety
- ESLint for code quality
- Modular architecture
- Separation of concerns
- Error handling
- Logging

### Testing
- Unit tests available
- Integration tests available
- Manual testing procedures

---

## 🔗 Integration Points

1. **VS Code API**
   - Commands
   - Settings
   - File system
   - Output channels

2. **AI Services**
   - Anthropic Claude
   - OpenAI GPT
   - OpenRouter
   - Claude Code

3. **External APIs**
   - Supabase (Email)
   - File system
   - Shell execution

---

## 📚 Documentation

### Available Docs
- `FEATURES_OVERVIEW.md` - Detailed features
- `ARCHITECTURE_DIAGRAM.md` - System architecture
- `QUICK_REFERENCE.md` - Quick reference
- `CODEBASE_SUMMARY.md` - This file

---

## 🎯 Development Workflow

### Adding New Feature
1. Create type definitions in `src/types/`
2. Implement service logic in `src/services/`
3. Create React component in `src/webview/components/`
4. Add tool if needed in `src/tools/`
5. Register command in `src/extension.ts`
6. Update documentation

### Adding New Tool
1. Create tool file in `src/tools/`
2. Implement tool interface
3. Add to CustomAgentService
4. Test with AI agent
5. Document usage

---

## 🚀 Future Enhancements

1. **Element Click Detection** - Detect HTML element clicks in iframe
2. **Design Collaboration** - Share designs with team
3. **Version Control** - Git integration
4. **Export Options** - Export to Figma, React
5. **Real-time Preview** - Live preview with hot reload
6. **Component Library** - Reusable components
7. **Design System** - Centralized tokens
8. **Analytics** - Usage tracking

---

## 📞 Support & Resources

- **Repository:** GitHub (superdesigndev/superdesign)
- **Issues:** GitHub Issues
- **Documentation:** In-repo markdown files
- **Version:** 0.0.14
- **Last Updated:** December 28, 2025

---

## 🎓 Learning Path

### For New Developers
1. Start with `QUICK_REFERENCE.md`
2. Read `ARCHITECTURE_DIAGRAM.md`
3. Explore `src/extension.ts`
4. Study `src/webview/App.tsx`
5. Review specific components
6. Check `src/services/` for business logic
7. Understand `src/tools/` for AI integration

### For Contributors
1. Fork repository
2. Create feature branch
3. Follow code style (ESLint)
4. Add tests
5. Update documentation
6. Submit pull request

---

**Last Updated:** December 28, 2025  
**Version:** 0.0.14  
**Status:** Active Development  
**Maintainer:** SuperdesignDev
