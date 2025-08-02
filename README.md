# MCP Windows Setup That Actually Works

> **Browser automation + file management through AI agents on Windows. No Docker, no WSL, no headaches.**

```bash
# 5 minutes to working MCP agents
git clone https://github.com/SelviAkdag/MCP.git
cd MCP
python -m venv venv 
venv\Scripts\activate 
npm install -g npx
pip install "huggingface_hub[mcp]>=0.32.0"
huggingface-cli login
npm install @playwright/test @modelcontextprotocol/server-filesystem
npx playwright install
tiny-agents run agents/agent_browser.json
# → "Take a screenshot of google.com" → ✅ Works
```

## What You Get

**Two production-ready MCP agents:**
- 🌐 **Browser Agent**: 24 Playwright tools (screenshots, navigation, form filling)
- 📁 **Filesystem Agent**: File operations (read, write, search, organize)
- 🪟 **Windows Native**: No virtualization, runs on bare Windows

**Real commands that work:**
```bash
"Take a screenshot of google.com"           # → Saves actual .jpg file
"Search for 'python tutorials' on Google"  # → Performs real search  
"List all .json files in my Documents"     # → Returns file paths
"Create a file called notes.txt"           # → Actually creates file
```

## Problem This Solves

**Before:** MCP tutorials assume Linux/Mac. Windows users hit:
- ❌ `KeyError: 'command'` 
- ❌ `Connection closed`
- ❌ `Chrome not found` 
- ❌ PowerShell execution policy blocks

**After:** Copy-paste configs that work on Windows. Zero modifications needed.

## Quick Start

**Prerequisites:** Node.js 18+, Python 3.8+, Hugging Face account

**Setup (5 minutes):**
```bash
# 1. Create virtual environment (recommended)
python -m venv venv

# 2. Activate virtual environment
venv\Scripts\activate

# 3. Install npx globally (if not already installed)
npm install -g npx

# 4. Install Hugging Face Hub with MCP support
pip install "huggingface_hub[mcp]>=0.32.0"

# 5. Login to Hugging Face (requires token)
huggingface-cli login

# 6. Install MCP servers
npm install @playwright/test @modelcontextprotocol/server-filesystem

# 7. Install browsers (requires admin prompt - one time only)
npx playwright install

# 8. Run browser agent
tiny-agents run agents/agent_browser.json

# 9. Test browser automation
# Type: "Take a screenshot of google.com"

# 10. Try filesystem agent (in new terminal)
tiny-agents run agents/agent_filesystem.json

# 11. Test file operations  
# Type: "List files in my Documents folder"
```

**If step 7 fails:** See [browser agent troubleshooting](docs/agent_browser.md)
**If filesystem issues:** See [filesystem agent guide](docs/agent_filesystem.md)
**If step 5 fails:** You need a [Hugging Face account and token](https://huggingface.co/settings/tokens)

## Repository Structure

```
agents/
├── agent_browser.json      # Browser automation (Playwright)
└── agent_filesystem.json   # File operations  

docs/
├── agent_browser.md        # Browser agent setup guide with troubleshooting
└── agent_filesystem.md     # Filesystem agent setup guide

examples/
└── test-commands.md        # Copy-paste test commands
```

## Windows-Specific Solutions

This repo documents solutions for:

| Issue | Solution |
|-------|----------|
| `KeyError: 'command'` | Flatten JSON config structure |
| `Connection closed` | Use `cmd /c` wrapper for npx |
| Chrome install fails | Run as Administrator, use Command Prompt |
| PowerShell blocks npm | Switch to Command Prompt |
| Path spaces break things | Quote paths with spaces |

**Full troubleshooting:** [docs/agent_browser.md](docs/agent_browser.md) (browser) | [docs/agent_filesystem.md](docs/agent_filesystem.md) (filesystem)

## Example Usage

**Browser automation:**
```javascript
// What happens behind the scenes:
await page.screenshot({
  path: 'C:\\Users\\...\\google-screenshot.jpg',
  quality: 50,
  type: 'jpeg'
});
```

**File operations:**
```bash
"Show me my Documents folder"
# Returns:
# [DIR] Projects
# [FILE] resume.pdf  
# [DIR] MCP
# [FILE] notes.txt
```

## Why This Exists

- **MCP is powerful** but setup docs assume Unix-like systems
- **Windows developers** deserve working examples
- **Real troubleshooting** from actual Windows setup experience
- **Copy-paste solutions** instead of "it should work" advice

## Contributing

Found a Windows-specific issue? **Please open an issue** with:
- Your Windows version
- Error message (full traceback)
- What you were trying to do

## License

MIT - Use this however helps you get MCP working on Windows.

---

**⚡ TL;DR:** `git clone https://github.com/SelviAkdag/MCP.git` → `cd MCP` → `npm install -g npx` → `pip install "huggingface_hub[mcp]>=0.32.0"` → `huggingface-cli login` → `npm install @playwright/test @modelcontextprotocol/server-filesystem` → `npx playwright install` → `tiny-agents run agents/agent_browser.json` → Type `"Take a screenshot of google.com"` → It works.