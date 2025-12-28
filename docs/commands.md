# Commands, Menus, Activation, and Views

Activation events:
- onCommand: superdesign.helloWorld
- onView: superdesign.chatView

Contributed commands:
- superdesign.helloWorld – Hello World
- superdesign.configureApiKey – Configure Anthropic API Key
- superdesign.configureOpenAIApiKey – Configure OpenAI API Key
- superdesign.configureOpenAIUrl – Configure OpenAI url
- superdesign.configureOpenRouterApiKey – Configure OpenRouter API Key
- superdesign.showChatSidebar – Show Chat Sidebar
- superdesign.openCanvas – Open Canvas View
- superdesign.clearChat – Clear Chat
- superdesign.resetWelcome – Reset Welcome Screen
- superdesign.initializeProject – Initialize Superdesign
- superdesign.openSettings – Open Settings

Menus (view/title):
- When view == superdesign.chatView
  - superdesign.openCanvas (navigation)
  - superdesign.openSettings (navigation)
  - superdesign.clearChat (navigation)

Views:
- viewsContainers.activitybar: id superdesign-sidebar, title "Superdesign", icon icon.svg
- views.superdesign-sidebar: webview id superdesign.chatView, name "SUPER DESIGN"

How to open UI:
- Superdesign: Show Chat Sidebar – focuses the sidebar view
- Superdesign: Open Canvas View – opens a panel webview rendering the Canvas
