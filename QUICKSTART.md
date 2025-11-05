# 🚀 Quick Start Guide

Get your Databricks MCP server running!

## Prerequisites

Before you begin, make sure you have:

- ✅ **Python 3.10+** installed (`python --version`)
- ✅ **Git** installed
- ✅ **Cursor IDE** or another MCP-compatible client
- ✅ **Databricks workspace** with access credentials

---

## Step 1: Clone & Install

```bash
# Clone the repository
git clone https://github.com/THEAVINASHREDDY/databricks-mcp.git
cd databricks-mcp

# Install the package in editable mode
pip install -e .
```

**That's it!** The package and all dependencies are now installed.

---

## Step 2: Get Your Databricks Credentials

You need three pieces of information:

1. **Workspace URL** - Your Databricks workspace URL
   - Example: `https://dbc-12345678-abcd.cloud.databricks.com`

2. **Access Token** - Personal access token
   - Get it from: Databricks → User Settings → Developer → Access Tokens → Generate New Token

3. **Warehouse ID** (Optional) - For SQL queries
   - Find at: SQL Warehouses → Your Warehouse → Connection Details

---

## Step 3: Configure Cursor

### Find Your Project Path

```bash
# Mac/Linux:
pwd

# Windows (PowerShell):
Get-Location
```

Copy this path (e.g., `/Users/yourname/databricks-mcp`)

### Update MCP Configuration

1. Open `~/.cursor/mcp.json` (create if it doesn't exist)
   - **Mac/Linux:** `/Users/yourname/.cursor/mcp.json`
   - **Windows:** `C:\Users\YourName\.cursor\mcp.json`

2. Add this configuration:

```json
{
  "mcpServers": {
    "databricks-mcp": {
      "command": "python",
      "args": [
        "-m",
        "databricks_mcp.server.databricks_mcp_server"
      ],
      "env": {
        "DATABRICKS_HOST": "https://your-workspace.databricks.com",
        "DATABRICKS_TOKEN": "dapiXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        "DATABRICKS_WAREHOUSE_ID": "sql_warehouse_xxxxx",
        "PYTHONPATH": "/absolute/path/to/databricks-mcp"
      }
    }
  }
}
```

3. **Replace the values:**
   - `DATABRICKS_HOST` → Your workspace URL
   - `DATABRICKS_TOKEN` → Your access token
   - `DATABRICKS_WAREHOUSE_ID` → Your warehouse ID (optional)
   - `PYTHONPATH` → The path you copied earlier

4. **Save the file**

---

## Step 4: Test!

1. **Restart Cursor completely**
2. Open Cursor chat
3. Try: **"List my Databricks clusters"**

✅ If you see your clusters, you're all set! 🎉

---

## ⚡ Quick Troubleshooting

### Error: "No module named 'databricks_mcp'"

**Fix:** Make sure you ran `pip install -e .` in the project directory

### Error: "Authentication failed"

**Fix:** Double-check your `DATABRICKS_TOKEN` and `DATABRICKS_HOST`

### Tools not showing in Cursor

**Fix:** 
1. Verify `mcp.json` has no syntax errors
2. Completely restart Cursor (not just reload)
3. Check logs: View → Output → Select "MCP"

---

## 🎯 What Can You Do Now?

Try these commands in Cursor:

- **"List my Databricks clusters"**
- **"Show my Databricks jobs"**
- **"List notebooks in /Users/myname"**
- **"Execute this SQL: SELECT * FROM my_table LIMIT 10"**
- **"What Databricks tools are available?"**

---

## 📚 Need More Help?

- **Detailed Instructions:** See [INSTALLATION.md](INSTALLATION.md)
- **Troubleshooting:** See [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- **Full Documentation:** See [README.md](README.md)
- **Bug Fixes:** See [FIXES_SUMMARY.md](FIXES_SUMMARY.md)

---

## 🔄 Updating

When updates are available:

```bash
cd databricks-mcp
git pull origin master
# No reinstall needed! Just restart Cursor
```

---

**Need help?** [Open an issue](https://github.com/THEAVINASHREDDY/databricks-mcp/issues) on GitHub! 🚀

