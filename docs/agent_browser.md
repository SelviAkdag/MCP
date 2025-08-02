# MCP Agent Setup Guide - Playwright Browser Tools

## Overview
This guide documents the step-by-step process of successfully setting up a Model Context Protocol (MCP) agent with Playwright browser tools on Windows, including all problems encountered and their solutions.

## Initial Setup

### Environment
- **OS:** Windows with MINGW64 terminal
- **Python:** 3.12 with virtual environment
- **Node.js:** v22.14.0
- **npm:** 10.9.2
- **Course:** Hugging Face MCP Tutorial

### Project Structure
```
C:\Users\YourUsername\Documents\MCP\
├── venv\
├── agents\
│   ├── agent_browser.json
│   └── agent_filesystem.json
├── docs\
│   ├── agent_browser.md
│   └── agent_filesystem.md
└── (other files)
```

## Step-by-Step Resolution Process

### Step 1: Initial Configuration Attempt
**Command:**
```bash
tiny-agents run agents/agent_browser.json
```

**Initial Configuration (`agents/agent_browser.json`):**
```json
{
    "model": "Qwen/Qwen2.5-72B-Instruct",
    "provider": "nebius",
    "servers": [
        {
            "type": "stdio",
            "config": {
                "command": "npx",
                "args": ["@playwright/mcp@latest"]
            }
        }
    ]
}
```

**Error Encountered:**
```
KeyError: 'command'
Traceback shows: params['command'] not found
```

**Root Cause:** The huggingface_hub MCP client expected `command` at the top level, not nested under `config`.

**Solution:** Flatten the configuration structure
```json
{
    "model": "Qwen/Qwen2.5-72B-Instruct",
    "provider": "nebius",
    "servers": [
        {
            "type": "stdio",
            "command": "npx",
            "args": ["@playwright/mcp@latest"]
        }
    ]
}
```

### Step 2: Connection Issues
**Error Encountered:**
```
McpError: Connection closed
```

**Root Cause:** On Windows, `npx` commands need to be executed through `cmd` to work properly with MCP's stdio communication.

**Solution:** Modified configuration to use Windows-compatible command execution
```json
{
    "model": "Qwen/Qwen2.5-72B-Instruct",
    "provider": "nebius",
    "servers": [
        {
            "type": "stdio",
            "command": "cmd",
            "args": ["/c", "npx", "@playwright/mcp@latest"]
        }
    ]
}
```

**Verification Command:**
```bash
tiny-agents run agents/agent_browser.json
```

**Result:** ✅ Success! Agent loaded with 24 tools:
```
Agent loaded with 24 tools:
• browser_close
• browser_resize  
• browser_console_messages
• browser_handle_dialog
• browser_evaluate
• browser_file_upload
• browser_install
• browser_press_key
• browser_type
• browser_navigate
• browser_navigate_back
• browser_navigate_forward
• browser_network_requests
• browser_take_screenshot
• browser_snapshot
• browser_click
• browser_drag
• browser_hover
• browser_select_option
• browser_tab_list
• browser_tab_new
• browser_tab_select
• browser_tab_close
• browser_wait_for
```

### Step 3: Browser Installation Issues
**Test Command:**
```
"Take a screenshot of google.com"
```

**Error Encountered:**
```
browserType.launchPersistentContext: Chromium distribution 'chrome' is not found
Run "npx playwright install chrome"
```

**Root Cause:** Playwright browsers were not installed on the system.

**Initial Attempts (Failed):**
```bash
# These failed due to insufficient privileges
npx playwright install chrome
npm install @playwright/test
npx playwright install
```

**PowerShell Execution Policy Issues:**
When attempting to run npm commands in PowerShell as Administrator:
```
npm : File C:\Program Files\nodejs\npm.ps1 cannot be loaded because running scripts is disabled
```

**Solution:** Used Command Prompt instead of PowerShell to avoid execution policy restrictions.

### Step 4: Successful Browser Installation
**Process:**
1. **Opened Command Prompt as Administrator:**
   - Method: Windows Key + R → `cmd` → Ctrl + Shift + Enter → Yes

2. **Navigated to project directory:**
   ```cmd
   cd "C:\Users\YourUsername\Documents\MCP"
   ```
   *Note: Quotes needed due to space in username (if applicable)*

3. **Activated virtual environment:**
   ```cmd
   venv\Scripts\activate
   ```

4. **Installed Playwright test package:**
   ```cmd
   npm install @playwright/test
   ```

5. **Force-installed Chrome browser:**
   ```cmd
   npx playwright install chrome --force
   ```

**Alternative commands tried:**
```bash
# Check available browsers
npx playwright install --dry-run

# Install all browsers
npx playwright install

# Force reinstall
npx playwright install --force
```

### Step 5: Final Testing and Success
**Test Process:**
1. **Started agent:**
   ```bash
   tiny-agents run agents/agent_browser.json
   ```

2. **Executed screenshot command:**
   ```
   "Take a screenshot of google.com"
   ```

**Successful Result:**
```javascript
// Screenshot viewport and save it as
C:\Users\USERNAME~1\AppData\Local\Temp\playwright-mcp-output\2025-07-28T19-10-23.370Z\google-startpage.jpg
await page.screenshot({
  path: 'C:\\Users\\USERNAME~1\\AppData\\Local\\Temp\\playwright-mcp-output\\2025-07-28T19-10-23.370Z\\google-startpage.jpg',
  quality: 50,
  scale: 'css',
  type: 'jpeg'
});
```

## Final Working Configuration

```json
{
    "model": "Qwen/Qwen2.5-72B-Instruct",
    "provider": "nebius",
    "servers": [
        {
            "type": "stdio",
            "command": "cmd",
            "args": ["/c", "npx", "@playwright/mcp@latest"]
        }
    ]
}
```

## Key Problems and Solutions Summary

| Problem | Root Cause | Solution |
|---------|------------|----------|
| `KeyError: 'command'` | Nested config structure | Flatten JSON structure |
| `Connection closed` | Windows npx compatibility | Use `cmd /c` wrapper |
| Browser not found | Missing Playwright browsers | Install as Administrator |
| PowerShell script execution | Execution policy restrictions | Use Command Prompt instead |
| Path issues | Spaces in username | Use quotes around paths |

## Critical Windows-Specific Learnings

1. **Administrator Rights Required:** Browser installation requires elevated privileges
2. **Command Prompt > PowerShell:** Avoids execution policy issues
3. **Path Handling:** Spaces in usernames require quoted paths
4. **MCP Communication:** stdio servers need `cmd /c` wrapper on Windows
5. **Username Translation:** Windows converts usernames with spaces to short format (e.g., "USERNAME~1") in temp paths

## Available Tools After Setup

The successfully configured agent provides 24 browser automation tools:
- Navigation: `browser_navigate`, `browser_navigate_back`, `browser_navigate_forward`
- Interaction: `browser_click`, `browser_type`, `browser_hover`, `browser_drag`
- Screenshots: `browser_take_screenshot`, `browser_snapshot`
- Tab management: `browser_tab_new`, `browser_tab_close`, `browser_tab_select`
- Advanced: `browser_evaluate`, `browser_file_upload`, `browser_wait_for`
- And more...

## Testing Commands

After successful setup, these commands work:
- `"Take a screenshot of google.com"`
- `"Navigate to github.com and tell me what you see"`
- `"Search for 'MCP Model Context Protocol' on Google"`
- `"Go to wikipedia.org and click on a random article"`

## Screenshot Location

Screenshots are saved to:
```
C:\Users\USERNAME~1\AppData\Local\Temp\playwright-mcp-output\[timestamp]\[filename].jpg
```

Access via File Explorer by pasting the path in the address bar.

## Conclusion

The key to success was understanding that:
1. MCP configuration format differs from standard Playwright setup
2. Windows requires specific command wrappers for stdio communication
3. Administrator privileges are essential for browser installation
4. Command Prompt is more reliable than PowerShell for npm operations

This setup now provides a fully functional MCP agent capable of browser automation tasks.