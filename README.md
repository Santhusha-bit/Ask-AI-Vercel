# Ask Claude Anywhere

A powerful Chrome extension that brings Claude AI directly to your browsing experience. Highlight any text on any webpage and get instant AI assistance without leaving the page.

## ✨ Features

- **🎯 Highlight & Ask** - Select any text and get instant Claude responses
- **🖱️ Context Menu Integration** - Right-click selected text for quick access
- **🎨 Beautiful Sidebar UI** - Clean, non-intrusive interface that slides from the right
- **⚡ Quick Actions** - Pre-built buttons for common tasks (Explain, Summarize, Simplify)
- **🔒 Secure Backend** - API key safely stored on Vercel backend, never exposed in browser
- **🌐 Works Everywhere** - Compatible with any website (except Chrome internal pages)
- **⌨️ Keyboard Shortcuts** - Press Enter to send, Shift+Enter for new line, Esc to close
- **💾 Conversation History** - Keep track of your queries within a session

## 🚀 Quick Start

### Installation

1. **Get the extension:**
   - Clone or download this repository
   - Or search "Ask Claude Anywhere" in the Chrome Web Store

2. **Load it locally (development):**
   - Go to `chrome://extensions/`
   - Enable "Developer Mode" (top right)
   - Click "Load unpacked"
   - Select the extension folder

3. **Use it:**
   - Highlight any text on a webpage
   - Click "Ask Claude" button or right-click → "Ask Claude"
   - Type your question and hit Enter
   - Get instant responses powered by Claude AI

## 🏗️ Architecture

This extension uses a **backend-first architecture** for security:

```
User's Chrome Browser
        ↓
    Extension UI
        ↓
  HTTPS Request
        ↓
Vercel Serverless Function
        ↓
  Claude API
        ↓
    Response
        ↓
    Extension UI
```

**Why this design?**
- ✅ API key never exposed to users
- ✅ Secure server-side validation
- ✅ Easy to update backend without requiring extension updates
- ✅ Can track usage and add features like rate limiting
- ✅ Scalable and reliable with Vercel

## 📋 Requirements

- **Browser:** Chrome, Edge, or any Chromium-based browser
- **Backend:** Deployed Vercel instance with Claude API key configured

## 🔧 Development Setup

### Prerequisites

- Node.js 14+
- A Claude API key from [Anthropic](https://console.anthropic.com)
- A GitHub account
- A Vercel account

### Backend Setup

1. **Prepare your backend:**
   ```bash
   # Create your api folder with these files:
   api/
   ├── ask.js       # Main API endpoint
   ├── test.js      # Testing endpoint
   └── .env.local   # Local environment variables (not pushed)
   ```

2. **Deploy to Vercel:**
   - Push your repo to GitHub
   - Visit [Vercel Dashboard](https://vercel.com/dashboard)
   - Click "Add New..." → "Project"
   - Select your GitHub repository
   - Set environment variable: `CLAUDE_API_KEY` = `your-api-key`
   - Click "Deploy"

3. **Get your URL:**
   - Vercel provides a URL like: `https://your-project.vercel.app`
   - Your API endpoint: `https://your-project.vercel.app/api/ask`

### Extension Setup

1. **Update configuration:**
   - Open `popup.js` and `content.js`
   - Replace `BACKEND_URL` with your Vercel URL:
   ```javascript
   const BACKEND_URL = 'https://your-project.vercel.app';
   ```

2. **Load in Chrome:**
   ```bash
   # Using command line
   google-chrome --load-extension=/path/to/extension
   
   # Or manually:
   1. Go to chrome://extensions/
   2. Enable Developer Mode
   3. Click "Load unpacked"
   4. Select extension folder
   ```

3. **Test:**
   - Go to any website
   - Highlight some text
   - Click "Ask Claude" button
   - Verify it works

## 🌐 API Documentation

### POST `/api/ask`

**Request:**
```json
{
  "query": "What does this mean?",
  "selectedText": "The quick brown fox...",
  "context": {
    "url": "https://example.com",
    "title": "Example Page"
  }
}
```

**Response:**
```json
{
  "success": true,
  "response": "Claude's detailed response here...",
  "tokens": {
    "input": 150,
    "output": 200
  }
}
```

**Error Response:**
```json
{
  "success": false,
  "error": "Error message describing what went wrong",
  "code": "ERROR_CODE"
}
```

### GET `/api/test`

Simple health check endpoint. Returns:
```json
{
  "status": "OK",
  "message": "Backend is working!",
  "timestamp": "2025-11-02T12:00:00Z"
}
```

## 🔐 Security & Privacy

### How Your Data is Protected

- ✅ **No Storage:** Responses aren't stored on our servers
- ✅ **Encrypted:** All communication uses HTTPS
- ✅ **API Key Secure:** Key stored only on Vercel backend, never sent to browser
- ✅ **No Tracking:** We don't collect usage data or profiles
- ✅ **Open Source:** Code is transparent and auditable

### Privacy Policy

Your usage of Claude Anywhere is governed by:
1. [Anthropic's Privacy Policy](https://www.anthropic.com/privacy)
2. [Claude's Terms of Service](https://console.anthropic.com/terms)
3. [Chrome Web Store Privacy Policy](https://support.google.com/webstoreapps)

## 🐛 Troubleshooting

### Extension doesn't appear

**Problem:** No "Ask Claude" button appears when selecting text

**Solutions:**
1. Check that extension is enabled in `chrome://extensions/`
2. Refresh the webpage (Cmd+R / Ctrl+R)
3. Check browser console for errors (F12 → Console tab)
4. Try reloading the extension

### "Connection Failed" error

**Problem:** Extension can't reach the backend

**Solutions:**
1. Verify your Vercel URL is correct in `popup.js` and `content.js`
2. Check that your Vercel deployment is "Ready" (not failed)
3. Test the backend: Visit `https://your-project.vercel.app/api/test` in browser
4. Verify `CLAUDE_API_KEY` environment variable is set in Vercel

### "API Error" or no response

**Problem:** Backend is running but Claude API returns error

**Solutions:**
1. Verify API key is valid: Visit [Anthropic Console](https://console.anthropic.com)
2. Check you have API credits/plan active
3. Try again - might be temporary API issue
4. Check Vercel function logs for detailed error

### CORS errors in console

**Problem:** Browser console shows CORS-related error

**Solutions:**
1. This shouldn't happen with the provided backend code
2. Verify CORS headers are set in `api/ask.js`
3. Clear browser cache (Cmd+Shift+Delete / Ctrl+Shift+Delete)
4. Try in incognito/private mode

### High latency or slow responses

**Problem:** Taking too long to get responses

**Solutions:**
1. Check your internet connection
2. Verify Claude API isn't under heavy load
3. Vercel cold starts can take 2-5 seconds on first request
4. Try a different query (long contexts take longer)

## 📊 File Structure

```
ask-claude-anywhere/
├── manifest.json          # Extension manifest
├── popup.html            # Settings popup
├── popup.js              # Popup logic
├── content.js            # Content script (injected into pages)
├── background.js         # Background service worker
├── styles.css            # Extension styling
├── icons/
│   ├── icon-16.png
│   ├── icon-48.png
│   └── icon-128.png
└── api/                  # Backend (deployed separately to Vercel)
    ├── ask.js            # Main API endpoint
    ├── test.js           # Health check
    └── package.json
```

## 🚀 Deploying to Vercel

### Step-by-Step

1. **Prepare Repository:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

2. **Connect to Vercel:**
   - Go to https://vercel.com/dashboard
   - Click "Add New" → "Project"
   - Select your GitHub repo

3. **Configure Environment:**
   - During setup, add environment variables:
   - Name: `CLAUDE_API_KEY`
   - Value: `sk-ant-api03-...` (your actual key)
   - Framework: Select "Other"

4. **Deploy:**
   - Click "Deploy"
   - Wait for "Ready" status
   - Get your production URL

5. **Monitor:**
   - Check Vercel Dashboard for deployment status
   - View function logs for debugging
   - Monitor usage and performance

## 🧪 Testing

### Manual Testing Checklist

- [ ] Extension loads without errors in `chrome://extensions/`
- [ ] Highlight text appears on any website
- [ ] "Ask Claude" button appears after highlighting
- [ ] Right-click context menu shows "Ask Claude"
- [ ] Typing and sending queries works
- [ ] Responses appear in sidebar
- [ ] Multiple queries work in one session
- [ ] Clear history button works
- [ ] Works on news sites, documentation, blogs, etc.

### Testing with Console

```javascript
// Test API endpoint directly
fetch('https://your-project.vercel.app/api/test')
  .then(r => r.json())
  .then(d => console.log(d));

// Test with query
fetch('https://your-project.vercel.app/api/ask', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ query: 'Hello Claude!' })
})
  .then(r => r.json())
  .then(d => console.log(d));
```

## 🎯 Usage Tips

### Best Practices

1. **Be specific** - More context gives better responses
2. **Use for research** - Quickly understand unfamiliar topics
3. **Code help** - Ask Claude to explain or optimize code snippets
4. **Writing assistance** - Get feedback on your writing
5. **Summarization** - Summarize long articles instantly

### Keyboard Shortcuts

- **Enter** - Send message
- **Shift+Enter** - New line in message
- **Esc** - Close sidebar
- **Right-click** - Context menu on selected text

## 🤝 Contributing

Contributions welcome! To contribute:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📝 License

MIT License - See LICENSE file for details

## 🆘 Support & Feedback

- **Report bugs:** [GitHub Issues](https://github.com/yourusername/ask-claude-anywhere/issues)
- **Feature requests:** [GitHub Discussions](https://github.com/yourusername/ask-claude-anywhere/discussions)
- **Email support:** support@example.com

## 🔗 Resources

- [Chrome Extension Documentation](https://developer.chrome.com/docs/extensions/)
- [Anthropic Claude API](https://console.anthropic.com/)
- [Vercel Documentation](https://vercel.com/docs)
- [Chrome Web Store Submission Guide](https://support.google.com/chrome/a/answer/2663860)

## 🙏 Acknowledgments

- Built with [Claude API](https://www.anthropic.com) for AI capabilities
- Hosted on [Vercel](https://vercel.com) for reliable serverless backend
- Chrome Extension framework from [Google Developers](https://developer.chrome.com)

---

**Version:** 1.0.0  
**Last Updated:** November 2025  
**Status:** Finised Development
