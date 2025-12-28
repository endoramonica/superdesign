# Webview UI and Messaging

Entry and routing:
- src/webview/index.tsx chooses between:
  - Chat view (panel or sidebar): needs WebviewContext injected as window.__WEBVIEW_CONTEXT__
  - Canvas view (panel): no context injection needed; CanvasView communicates with extension

App component (src/webview/App.tsx):
- Acquires VS Code API via acquireVsCodeApi()
- Injects compiled CSS (App.css) at runtime
- Reads data-view and data-nonce from #root attributes
- Renders ChatInterface or CanvasView accordingly

Template (src/templates/webviewTemplate.ts):
- Injects WebviewContext and logo URIs into the window
- Uses VS Code webview CSP; loads a single bundled script dist/webview.js

DesignFrame (src/webview/components/DesignFrame.tsx):
- Core canvas HTML/CSS sandbox. If you need to interact with DOM inside an <iframe>, inject a script that posts messages to the parent window and bridge those to VS Code via the webview channel.

Example: instrumenting an iframe to capture element clicks
- In the webview (React) side, when mounting the iframe, inject a small content script via srcdoc or postMessage to the iframe contentWindow:

  1) Prepare a script string that registers a click listener and posts a message:
     const iframeScript = `
       (function(){
         function selectorFor(el){
           if (!el) return '';
           if (el.id) return '#' + el.id;
           const p = el.parentElement;
           const idx = p ? Array.prototype.indexOf.call(p.children, el) : 0;
           const tag = (el.tagName||'').toLowerCase();
           return (p ? selectorFor(p) + '>' : '') + tag + ':nth-child(' + (idx+1) + ')';
         }
         document.addEventListener('click', (e) => {
           const t = e.target;
           const payload = {
             type: 'iframe:element-click',
             tag: t && t.tagName,
             text: t && (t.innerText||t.textContent||''),
             selector: selectorFor(t)
           };
           window.parent.postMessage(payload, '*');
         }, true);
       })();`;

  2) Insert it into the iframe document when it is ready:
     const iframe = iframeRef.current;
     iframe.addEventListener('load', () => {
       try {
         const doc = iframe.contentDocument;
         const s = doc.createElement('script');
         s.type = 'text/javascript';
         s.textContent = iframeScript;
         doc.documentElement.appendChild(s);
       } catch (err) {
         console.error('Failed to inject iframe script', err);
       }
     });

  3) In the outer webview, listen for window message and forward to VS Code:
     window.addEventListener('message', (event) => {
       const msg = event.data;
       if (msg && msg.type === 'iframe:element-click') {
         vscode.postMessage({ type: 'designframe/element-click', payload: msg });
       }
     });

  4) In the extension host (src/extension.ts), handle the webview message and route to your feature logic.

Notes:
- For remote src iframes, browser security may block script injection; prefer using srcdoc or same-origin content.
- Always scope postMessage handling and consider origin validation for production.
