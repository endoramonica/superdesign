# 🎨 Superdesign - Tổng Hợp Chức Năng Hiện Có

## 📋 Tổng Quan Ứng Dụng

**Superdesign** là một VS Code Extension tích hợp AI Agent để thiết kế UI/Frontend trực tiếp trong IDE. Nó cho phép người dùng tạo, lặp lại và quản lý các thiết kế HTML/SVG với sự hỗ trợ của Claude AI.

---

## 🏗️ Kiến Trúc Hệ Thống

### Cấu Trúc Thư Mục

```
src/
├── extension.ts              # Entry point của VS Code Extension
├── assets/                   # Logo và icon (Cursor, Windsurf, Claude Code, Lovable, Bolt)
├── providers/                # VS Code Providers
│   ├── chatSidebarProvider.ts
│   ├── claudeApiProvider.ts
│   ├── claudeCodeProvider.ts
│   ├── llmProvider.ts
│   └── llmProviderFactory.ts
├── services/                 # Business Logic Services
│   ├── chatMessageService.ts
│   ├── claudeCodeService.ts
│   ├── customAgentService.ts
│   └── logger.ts
├── tools/                    # AI Agent Tools
│   ├── read-tool.ts
│   ├── write-tool.ts
│   ├── edit-tool.ts
│   ├── bash-tool.ts
│   ├── glob-tool.ts
│   ├── grep-tool.ts
│   ├── ls-tool.ts
│   ├── multiedit-tool.ts
│   ├── theme-tool.ts
│   └── tool-utils.ts
├── types/                    # TypeScript Type Definitions
│   ├── agent.ts
│   └── context.ts
├── templates/                # Webview HTML Templates
│   └── webviewTemplate.ts
└── webview/                  # React Frontend
    ├── App.tsx
    ├── App.css
    ├── index.tsx
    ├── components/
    │   ├── CanvasView.tsx
    │   ├── DesignFrame.tsx
    │   ├── DesignPanel.tsx
    │   ├── ConnectionLines.tsx
    │   ├── Icons.tsx
    │   ├── MarkdownRenderer.tsx
    │   ├── Chat/
    │   │   └── ChatInterface.tsx
    │   └── Welcome/
    ├── hooks/
    │   ├── useChat.ts
    │   └── useFirstTimeUser.ts
    ├── types/
    │   └── canvas.types.ts
    └── utils/
        ├── gridLayout.ts
        └── themeParser.ts
```

---

## 🎯 Chức Năng Chính

### 1. **Chat Interface (💬 Giao Diện Chat)**

**File:** `src/webview/components/Chat/ChatInterface.tsx`

**Chức năng:**
- Giao diện chat tương tác với AI Agent
- Gửi tin nhắn và nhận phản hồi từ Claude AI
- Hiển thị lịch sử chat
- Hỗ trợ Markdown rendering cho phản hồi
- Tích hợp với các AI Provider khác nhau

**Tính năng:**
- ✅ Real-time chat streaming
- ✅ Markdown support với syntax highlighting
- ✅ Message history
- ✅ Stop chat functionality
- ✅ Error handling và retry logic

---

### 2. **Canvas View (🎨 Xem Thiết Kế)**

**File:** `src/webview/components/CanvasView.tsx`

**Chức năng:**
- Hiển thị các file thiết kế (HTML/SVG) dưới dạng grid
- Quản lý vị trí và kích thước các frame thiết kế
- Zoom và pan trên canvas
- Kết nối các thiết kế với connection lines
- Drag & drop các frame

**Tính năng:**
- ✅ Grid layout system
- ✅ Zoom in/out
- ✅ Pan functionality
- ✅ Frame selection
- ✅ Connection visualization
- ✅ Responsive design

---

### 3. **Design Frame Component (🖼️ Khung Thiết Kế)**

**File:** `src/webview/components/DesignFrame.tsx`

**Chức năng:**
- Hiển thị từng file thiết kế HTML/SVG
- Render content trong iframe
- Quản lý viewport (Mobile, Tablet, Desktop)
- Floating action buttons

**Tính năng:**
- ✅ Iframe rendering với CSP security
- ✅ SVG support
- ✅ Viewport switching (Mobile/Tablet/Desktop)
- ✅ Copy prompt to clipboard (cho Cursor, Windsurf, Claude Code, Lovable, Bolt)
- ✅ Copy design path
- ✅ Create variations
- ✅ Iterate with feedback
- ✅ Drag & drop support
- ✅ Loading/Error states
- ✅ Service Worker for external resources

---

### 4. **AI Agent Service (🤖 Dịch Vụ AI Agent)**

**File:** `src/services/customAgentService.ts`

**Chức năng:**
- Quản lý AI model providers (Anthropic, OpenAI, OpenRouter, Claude Code)
- Thực thi các tool commands
- Xử lý streaming responses

**Hỗ trợ Providers:**
- ✅ Anthropic Claude API
- ✅ OpenAI API
- ✅ OpenRouter API
- ✅ Claude Code (local binary)

---

### 5. **AI Tools (🛠️ Công Cụ AI)**

Các tool được AI Agent sử dụng để thao tác với file system:

#### **Read Tool** (`read-tool.ts`)
- Đọc nội dung file
- Hỗ trợ line range
- Xử lý multiple files

#### **Write Tool** (`write-tool.ts`)
- Tạo/ghi file mới
- Tự động tạo parent directories
- Hỗ trợ multiple files

#### **Edit Tool** (`edit-tool.ts`)
- Thay thế text trong file
- Exact string matching
- Preserve formatting

#### **Multiedit Tool** (`multiedit-tool.ts`)
- Thực hiện multiple find-and-replace operations
- Sequential execution

#### **Bash Tool** (`bash-tool.ts`)
- Thực thi shell commands
- Timeout support
- Output capture

#### **Glob Tool** (`glob-tool.ts`)
- Tìm files matching glob patterns
- Efficient file discovery

#### **Grep Tool** (`grep-tool.ts`)
- Tìm text patterns trong files
- Regex support
- Context lines

#### **LS Tool** (`ls-tool.ts`)
- Liệt kê directory contents
- Filtering & sorting
- Detailed information

#### **Theme Tool** (`theme-tool.ts`)
- Tạo CSS theme files
- Lưu theme vào `.superdesign/design_iterations/`

---

### 6. **Project Initialization (📁 Khởi Tạo Dự Án)**

**File:** `src/extension.ts` - `initializeSuperdesignProject()`

**Chức năng:**
- Tạo `.superdesign/` folder structure
- Tạo design rules file
- Tạo default CSS theme
- Setup project templates

**Tạo ra:**
- ✅ `.superdesign/` directory
- ✅ `.superdesign/design_iterations/` folder
- ✅ `.superdesign/moodboard/` folder
- ✅ Design rules file
- ✅ Default CSS theme

---

### 7. **Image Management (🖼️ Quản Lý Hình Ảnh)**

**File:** `src/extension.ts`

**Chức năng:**
- Lưu uploaded images vào moodboard
- Convert images to base64
- Hỗ trợ multiple image formats

**Tính năng:**
- ✅ Save images to `.superdesign/moodboard/`
- ✅ Base64 conversion
- ✅ MIME type detection
- ✅ File size tracking

**Hỗ trợ Format:**
- JPG/JPEG
- PNG
- GIF
- WebP
- BMP

---

### 8. **CSS File Management (🎨 Quản Lý CSS)**

**File:** `src/extension.ts` - `getCssFileContent()`

**Chức năng:**
- Đọc CSS file content
- Hỗ trợ relative paths
- Theme preview

---

### 9. **Email Submission (📧 Gửi Email)**

**File:** `src/extension.ts` - `submitEmailToSupabase()`

**Chức năng:**
- Gửi email đến Supabase API
- Hỗ trợ form submission
- Error handling

---

### 10. **LLM Provider Factory (🏭 Nhà Máy Provider)**

**File:** `src/providers/llmProviderFactory.ts`

**Chức năng:**
- Tạo LLM provider instances
- Quản lý provider configuration
- Support multiple providers

**Providers:**
- ✅ Anthropic Claude
- ✅ OpenAI
- ✅ OpenRouter
- ✅ Claude Code

---

### 11. **Chat Message Service (💬 Dịch Vụ Tin Nhắn Chat)**

**File:** `src/services/chatMessageService.ts`

**Chức năng:**
- Xử lý chat messages
- Streaming responses
- Tool execution
- Error handling

---

### 12. **Logger Service (📝 Dịch Vụ Ghi Log)**

**File:** `src/services/logger.ts`

**Chức năng:**
- Centralized logging
- Multiple log levels
- Output channel integration

**Log Levels:**
- ✅ INFO
- ✅ WARN
- ✅ ERROR
- ✅ DEBUG

---

### 13. **Design File Management (📄 Quản Lý File Thiết Kế)**

**Chức năng:**
- Quét `.superdesign/design_iterations/` folder
- Tải design files
- Watch file changes
- Hiển thị file hierarchy

**Hỗ trợ:**
- ✅ HTML files
- ✅ SVG files
- ✅ File versioning
- ✅ Parent-child relationships

---

### 14. **Viewport Management (📱 Quản Lý Viewport)**

**Chức năng:**
- Switch giữa Mobile, Tablet, Desktop views
- Global viewport control
- Per-frame viewport control

**Viewport Modes:**
- ✅ Mobile (375px)
- ✅ Tablet (768px)
- ✅ Desktop (1920px)

---

### 15. **Floating Action Buttons (🎯 Nút Hành Động Nổi)**

**Chức năng:**
- Create variations
- Iterate with feedback
- Copy prompt to clipboard
- Copy design path

**Platforms:**
- ✅ Cursor
- ✅ Windsurf
- ✅ Claude Code
- ✅ Lovable
- ✅ Bolt

---

### 16. **Connection Lines (🔗 Đường Kết Nối)**

**File:** `src/webview/components/ConnectionLines.tsx`

**Chức năng:**
- Vẽ đường kết nối giữa các frame
- Hiển thị design hierarchy
- Visual relationship mapping

---

### 17. **Theme Parser (🎨 Phân Tích Theme)**

**File:** `src/webview/utils/themeParser.ts`

**Chức năng:**
- Parse CSS theme files
- Extract color variables
- Generate theme objects

---

### 18. **Grid Layout System (📐 Hệ Thống Grid Layout)**

**File:** `src/webview/utils/gridLayout.ts`

**Chức năng:**
- Auto-layout frames trên canvas
- Position calculation
- Collision detection

---

### 19. **Welcome Screen (👋 Màn Hình Chào Mừng)**

**File:** `src/webview/components/Welcome/`

**Chức năng:**
- First-time user experience
- Setup instructions
- API key configuration

---

### 20. **Markdown Renderer (📝 Renderer Markdown)**

**File:** `src/webview/components/MarkdownRenderer.tsx`

**Chức năng:**
- Render Markdown content
- Syntax highlighting
- Code block support

---

## ⚙️ Configuration Options

**Settings:**
```json
{
  "superdesign.llmProvider": "claude-api" | "claude-code",
  "superdesign.anthropicApiKey": "string",
  "superdesign.anthropicUrl": "string",
  "superdesign.claudeCodePath": "string",
  "superdesign.claudeCodeModelId": "string",
  "superdesign.claudeCodeThinkingBudget": "number",
  "superdesign.openaiApiKey": "string",
  "superdesign.openaiUrl": "string",
  "superdesign.openrouterApiKey": "string",
  "superdesign.aiModelProvider": "openai" | "anthropic" | "openrouter" | "claude-code",
  "superdesign.aiModel": "string"
}
```

---

## 🔄 Workflow Chính

### Design Creation Workflow

1. **User Input** → Chat Interface
2. **AI Processing** → Claude AI Agent
3. **Tool Execution** → Read/Write/Edit Tools
4. **File Generation** → `.superdesign/design_iterations/`
5. **Canvas Display** → CanvasView
6. **User Iteration** → Feedback Loop

---

## 📦 Dependencies

### Core Dependencies
- `vscode` - VS Code API
- `ai` - AI SDK
- `@ai-sdk/anthropic` - Anthropic provider
- `@ai-sdk/openai` - OpenAI provider
- `@openrouter/ai-sdk-provider` - OpenRouter provider
- `@anthropic-ai/claude-code` - Claude Code integration

### Frontend
- `react` - UI framework
- `react-dom` - React DOM
- `react-markdown` - Markdown rendering
- `react-zoom-pan-pinch` - Canvas zoom/pan
- `lucide-react` - Icons

### Utilities
- `glob` - File pattern matching
- `micromatch` - Glob matching
- `mime-types` - MIME type detection
- `highlight.js` - Code syntax highlighting
- `zod` - Schema validation

---

## 🎮 Commands

**Available Commands:**
- `superdesign.helloWorld` - Hello World
- `superdesign.configureApiKey` - Configure Anthropic API Key
- `superdesign.configureOpenAIApiKey` - Configure OpenAI API Key
- `superdesign.configureOpenAIUrl` - Configure OpenAI URL
- `superdesign.configureOpenRouterApiKey` - Configure OpenRouter API Key
- `superdesign.showChatSidebar` - Show Chat Sidebar
- `superdesign.openCanvas` - Open Canvas View
- `superdesign.clearChat` - Clear Chat
- `superdesign.resetWelcome` - Reset Welcome Screen
- `superdesign.initializeProject` - Initialize Superdesign
- `superdesign.openSettings` - Open Settings

---

## 🔐 Security Features

- ✅ Content Security Policy (CSP) for iframes
- ✅ Nonce injection for scripts
- ✅ Service Worker for external resources
- ✅ Secure message passing
- ✅ API key encryption in VS Code settings

---

## 📊 File Structure

### Design Files Location
```
workspace/
└── .superdesign/
    ├── design_iterations/
    │   ├── table_1.html
    │   ├── table_1_1.html
    │   ├── table_1_2.html
    │   ├── theme_1.css
    │   └── ...
    ├── moodboard/
    │   ├── image_1.jpg
    │   ├── image_2.png
    │   └── ...
    └── design_rules.mdc
```

---

## 🚀 Performance Features

- ✅ Lazy loading for iframes
- ✅ Efficient grid layout
- ✅ Zoom-based rendering
- ✅ Service Worker caching
- ✅ Streaming responses

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

---

## 📝 Notes

- Tất cả design files được lưu trong `.superdesign/design_iterations/`
- Hỗ trợ versioning thông qua naming convention: `design_name_n.html`
- Moodboard images được lưu trong `.superdesign/moodboard/`
- Theme CSS được tạo bởi AI Agent và lưu trong design_iterations
- Canvas view cho phép visualization của toàn bộ design hierarchy

---

## 🔗 Integration Points

- **VS Code API** - Extension lifecycle, commands, settings
- **Anthropic API** - Claude AI models
- **OpenAI API** - GPT models
- **OpenRouter API** - Multiple model providers
- **Supabase** - Email submission
- **File System** - Design file management
- **Webview** - UI rendering

---

**Last Updated:** December 28, 2025
**Version:** 0.0.14
