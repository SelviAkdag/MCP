# Test Commands for MCP Agents

This file contains copy-paste commands to verify your MCP agents are working correctly. Start with the basic tests and progress to more advanced ones.

## Browser Agent (agents/agent_browser.json)

### Prerequisites
- Agent started with: `tiny-agents run agents/agent_browser.json`
- Should show "Agent loaded with 24 tools"

### ✅ Basic Navigation Tests
**Copy-paste these commands exactly:**

```
Take a screenshot of google.com
```

```
Navigate to github.com and tell me what you see
```

```
Go to wikipedia.org
```

### ✅ Interactive Browser Tests
**Test user interactions:**

```
Search for 'Model Context Protocol' on Google
```

```
Go to stackoverflow.com and search for 'python'
```

```
Navigate to news.ycombinator.com and tell me the top 3 headlines
```

### ✅ Advanced Browser Operations
**More complex browser automation:**

```
Take screenshots of both google.com and bing.com
```

```
Go to github.com and click on the 'Sign up' button
```

```
Navigate to a weather website and tell me today's forecast
```

### 🔧 Troubleshooting Browser Commands
**If browser tests fail:**

```
Navigate to a simple webpage like example.com
```

```
Take a screenshot of example.com and save it
```

---

## Filesystem Agent (agents/agent_filesystem.json)

### Prerequisites
- Agent started with: `tiny-agents run agents/agent_filesystem.json`
- Should load filesystem tools successfully

### ✅ Basic File Operations
**Copy-paste these commands exactly:**

```
List files in the current directory
```

```
Show me what's in the Documents folder
```

```
Show me the files in the path C:\Users\YourUsername\Documents
```

### ✅ File Reading Tests
**Test file access:**

```
Read the content of agents/agent_browser.json file
```

```
Show me what's inside the README.md file
```

```
List all .json files in the current directory
```

### ✅ File Creation Tests
**Test file writing capabilities:**

```
Create a file named 'mcp-test.txt' with content 'MCP is working perfectly!'
```

```
Create a folder called 'mcp-experiments'
```

```
Create a file called 'status.md' with content '# MCP Setup Complete\n\nBoth agents are working!'
```

### 🔧 Troubleshooting Filesystem Commands
**If filesystem tests fail:**

```
List files in the MCP directory
```

```
Show me the current working directory
```

---

## Combined Testing Scenarios

### 📊 End-to-End Verification
**Test both agents are working:**

1. **Start browser agent:** `tiny-agents run agents/agent_browser.json`
2. **Test:** `Take a screenshot of google.com`
3. **Stop and switch:** Ctrl+C
4. **Start filesystem agent:** `tiny-agents run agents/agent_filesystem.json` 
5. **Test:** `Create a file named 'screenshot-success.txt' with content 'Browser screenshot completed!'`

### 🚀 Real-World Use Cases
**Practical examples:**

**Research Task (Browser Agent):**
```
Go to github.com, search for 'model context protocol', and tell me about the top 3 repositories
```

**File Organization (Filesystem Agent):**
```
Create a folder called 'research-notes' and then create a file inside it named 'mcp-findings.txt' with content 'MCP research started today'
```

**Web Data + File Storage (Manual Switch):**
```
# With browser agent:
Take a screenshot of a news website and tell me the main headline

# Switch to filesystem agent:
Create a file named 'daily-news.txt' with today's main headline as content
```

---

## Quick Diagnostic Commands

### 🔍 Is Everything Working?
**Run these if you're unsure about setup:**

**Browser Agent Health Check:**
```
Navigate to example.com and take a screenshot
```

**Filesystem Agent Health Check:**
```
List files in the current directory and create a file named 'health-check.txt'
```

### ⚠️ Common Issues and Test Commands

**If "Connection closed" error:**
- Make sure you're using the correct Windows configuration with `cmd /c`
- Test with: `Navigate to a simple page like example.com`

**If "Chrome not found" error:**
- Run: `npx playwright install chrome --force`
- Test with: `Take a screenshot of example.com`

**If filesystem access denied:**
- Check your paths don't have permission issues
- Test with: `List files in the current directory`

---

## Expected Results

### ✅ Successful Browser Commands Should:
- Navigate to websites without errors
- Take actual screenshot files (saved to temp directory)
- Interact with page elements (clicks, searches)
- Return descriptions of what's visible on pages

### ✅ Successful Filesystem Commands Should:
- List actual files and directories
- Create real files you can find in Windows Explorer
- Read existing file contents accurately
- Handle Windows paths with spaces correctly

### ❌ If Commands Fail:
- Check the specific error message
- Refer to troubleshooting guides in `docs/agent_browser.md` or `docs/agent_filesystem.md`
- Ensure prerequisites (Node.js, Python, HuggingFace login) are met
- Verify you're in the correct directory structure

---

## Next Steps

Once basic commands work:
1. **Experiment** with variations of successful commands
2. **Combine operations** (take screenshot, then save notes about it)
3. **Build workflows** using both agents for complex tasks
4. **Create your own** MCP servers for specialized needs

**Happy testing!** 🎉