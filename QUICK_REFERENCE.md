# ⚡ Superdesign - Quick Reference Guide

## 🎯 Chức Năng Chính (Top 20)

| # | Chức Năng | File | Mô Tả |
|---|-----------|------|-------|
| 1 | **Chat Interface** | `Chat/ChatInterface.tsx` | Giao diện chat với AI Agent |
| 2 | **Canvas View** | `CanvasView.tsx` | Hiển thị grid các design files |
| 3 | **Design Frame** | `DesignFrame.tsx` | Render HTML/SVG trong iframe |
| 4 | **AI Agent** | `customAgentService.ts` | Xử lý AI requests & tool execution |
| 5 | **LLM Providers** | `llmProviderFactory.ts` | Support Anthropic, OpenAI, OpenRouter, Claude Code |
| 6 | **Read Tool** | `read-tool.ts` | Đọc file contents |
| 7 | **Write Tool** | `write-tool.ts` | Tạo/ghi file mới |
| 8 | **Edit Tool** | `edit-tool.ts` | Thay thế text trong file |
| 9 | **Bash Tool** | `bash-tool.ts` | Thực thi shell commands |
| 10 | **Glob Tool** | `glob-tool.ts` | Tìm files matching patterns |
| 11 | **Grep Tool** | `grep-tool.ts` | Tìm text patterns |
| 12 | **Theme Tool** | `theme-tool.ts` | Tạo CSS theme files |
| 13 | **Viewport Control** | `DesignFrame.tsx` | Switch Mobile/Tablet/Desktop |
| 14 | **Copy Prompt** | `DesignFrame.tsx` | Copy design cho Cursor/Windsurf/Claude Code/Lovable/Bolt |
| 15 | **Image Management** | `extension.ts` | Lưu images vào moodboard |
| 16 | **Project Init** | `extension.ts` | Tạo `.superdesign/` structure |
| 17 | **Connection Lines** | `ConnectionLines.tsx` | Vẽ đường kết nối giữa frames |
| 18 | **Markdown Renderer** | `MarkdownRenderer.tsx` | Render markdown với syntax highlighting |
| 19 | **Grid Layout** | `gridLayout.ts` | Auto-layout frames trên canvas |
| 20 | **Logger Service** | `logger.ts` | Centralized logging |

---

## 📂 Folder Structure

```
.superdesign/
├── design_iterations/     ← Design files (HTML/SVG)
│   ├── table_1.html
│   ├── table_1_1.html
│   ├── theme_1.css
│   └── ...
├── moodboard/            ← Reference images
│   ├── image_1.jpg
│   └── ...
└── design_rules.mdc      ← AI design rules
```

---

## 🔧 Configuration

### Settings Keys
```json
{
  "superdesign.llmProvider": "claude-api" | "claude-code",
  "superdesign.anthropicApiKey": "sk-...",
  "superdesign.aiModelProvider": "anthropic" | "openai" | "openrouter" | "claude-code",
  "superdesign.aiModel": "claude-3-5-sonnet-20241022"
}
```

### Commands
```
superdesign.openCanvas              - Open Canvas View
superdesign.initializeProject       - Initialize Superdesign
superdesign.configureApiKey         - Configure API Key
superdesign.clearChat               - Clear Chat
superdesign.openSettings            - Open Settings
```

---

## 🎨 Component Hierarchy

```
App (Root)
├── ChatInterface
│   ├── MessageList
│   ├── InputField
│   └── MarkdownRenderer
├── CanvasView
│   ├── DesignFrame (multiple)
│   │   ├── iframe (HTML/SVG)
│   │   ├── FloatingActionButtons
│   │   └── ViewportControls
│   └── ConnectionLines
└── WelcomeScreen
    ├── SetupGuide
    └── ApiKeyInput
```

---

## 🔄 Message Flow

### Chat Message
```
User Input 
  → ChatMessageService 
  → LLM Provider 
  → AI Agent 
  → Tool Execution 
  → Response Stream 
  → Webview Display
```

### Design File Creation
```
User Request 
  → AI Agent 
  → Write Tool 
  → File System 
  → File Watcher 
  → Canvas Update
```

---

## 🛠️ AI Tools Summary

| Tool | Purpose | Example |
|------|---------|---------|
| **read** | Đọc file | `read(path: 'src/App.tsx')` |
| **write** | Tạo file | `write(path: '.superdesign/design.html', content: '...')` |
| **edit** | Sửa file | `edit(path: 'file.ts', oldStr: '...', newStr: '...')` |
| **multiedit** | Multiple edits | `multiedit(path: 'file.ts', edits: [...])` |
| **bash** | Shell command | `bash(command: 'npm install')` |
| **glob** | Find files | `glob(pattern: '**/*.tsx')` |
| **grep** | Search text | `grep(query: 'function', includePattern: '**/*.ts')` |
| **ls** | List directory | `ls(path: 'src', depth: 2)` |
| **theme** | Create theme | `generateTheme(name: 'Dark', cssPath: '...')` |

---

## 📊 File Types Supported

| Type | Support | Rendering |
|------|---------|-----------|
| **HTML** | ✅ Full | iframe with CSP |
| **SVG** | ✅ Full | iframe or direct |
| **CSS** | ✅ Read/Write | Theme generation |
| **Images** | ✅ JPG/PNG/GIF/WebP | Moodboard storage |

---

## 🎯 Viewport Sizes

| Mode | Width | Height |
|------|-------|--------|
| Mobile | 375px | 667px |
| Tablet | 768px | 1024px |
| Desktop | 1920px | 1080px |

---

## 🔐 Security Features

- ✅ CSP headers for iframes
- ✅ Nonce injection for scripts
- ✅ Service Worker for external resources
- ✅ API key encryption
- ✅ Workspace-scoped file access
- ✅ Message validation

---

## 📦 Key Dependencies

```json
{
  "vscode": "^1.90.0",
  "ai": "^4.3.16",
  "@ai-sdk/anthropic": "^1.2.12",
  "@ai-sdk/openai": "^1.3.22",
  "@openrouter/ai-sdk-provider": "^0.7.2",
  "react": "^19.1.0",
  "react-markdown": "^10.1.0",
  "react-zoom-pan-pinch": "^3.7.0",
  "highlight.js": "^11.11.1"
}
```

---

## 🚀 Performance Tips

- Lazy load iframes (zoom-based)
- Use Service Worker for external resources
- Efficient grid layout calculation
- Streaming responses for large outputs
- Debounce zoom/pan events

---

## 🐛 Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| API key not working | Check settings, re-enter key |
| Design not rendering | Check file format (HTML/SVG) |
| Slow canvas | Reduce number of frames, zoom out |
| Images not loading | Check CORS, use Service Worker |
| Chat not responding | Check API quota, network connection |

---

## 📝 Design File Naming Convention

```
{design_name}_{version}.html

Examples:
- table_1.html          (initial design)
- table_1_1.html        (first iteration)
- table_1_2.html        (second iteration)
- table_2.html          (new design)
- theme_1.css           (theme file)
```

---

## 🎨 Theme Variables

```css
:root {
  --background: oklch(...);
  --foreground: oklch(...);
  --primary: oklch(...);
  --secondary: oklch(...);
  --accent: oklch(...);
  --destructive: oklch(...);
  --border: oklch(...);
  --input: oklch(...);
  --ring: oklch(...);
  --font-sans: ...;
  --font-mono: ...;
  --radius: ...;
  --shadow-*: ...;
}
```

---

## 🔗 Integration Points

- **VS Code API** - Commands, settings, file system
- **Anthropic API** - Claude models
- **OpenAI API** - GPT models
- **OpenRouter API** - Multiple providers
- **Supabase** - Email submission
- **File System** - Design file management

---

## 📱 Responsive Design

- Mobile-first approach
- Flexbox & Grid layouts
- CSS variables for theming
- Viewport meta tags
- Media queries support

---

## 🎯 Next Steps for Development

1. **Element Click Detection** - Inject script to detect HTML element clicks
2. **Design Collaboration** - Share designs with team
3. **Version Control** - Git integration for design history
4. **Export Options** - Export to Figma, React components
5. **Real-time Preview** - Live preview with hot reload
6. **Component Library** - Reusable component management
7. **Design System** - Centralized design tokens
8. **Analytics** - Track design usage & performance

---

## 📚 Documentation Files

- `FEATURES_OVERVIEW.md` - Detailed feature documentation
- `ARCHITECTURE_DIAGRAM.md` - System architecture & diagrams
- `QUICK_REFERENCE.md` - This file

---

**Last Updated:** December 28, 2025  
**Version:** 0.0.14  
**Status:** Active Development
