# 🔧 Troubleshooting Guide

Common issues and their solutions when setting up Databricks MCP Server.

## Table of Contents

- [Installation Issues](#installation-issues)
- [Module Not Found Errors](#module-not-found-errors)
- [Authentication Issues](#authentication-issues)
- [Cursor Integration Issues](#cursor-integration-issues)
- [Connection Issues](#connection-issues)
- [Tool Execution Issues](#tool-execution-issues)
- [Logging and Debugging](#logging-and-debugging)

---

## Installation Issues

### ❌ Error: `pip: command not found`

**Problem:** pip is not installed or not in PATH

**Solution:**

```bash
# Mac/Linux - Install pip
python3 -m ensurepip --upgrade

# Or use your package manager
# Mac:
brew install python

# Ubuntu/Debian:
sudo apt install python3-pip

# Verify
pip --version
```

**Windows:**
- Reinstall Python from [python.org](https://www.python.org/)
- ✅ Check "Add Python to PATH" during installation

---

### ❌ Error: `permission denied` when installing

**Problem:** Insufficient permissions to install packages

**Solution 1:** Install for current user only
```bash
pip install --user -e .
```

**Solution 2:** Use a virtual environment (recommended)
```bash
# Create virtual environment
python -m venv .venv

# Activate it
# Mac/Linux:
source .venv/bin/activate

# Windows:
.\.venv\Scripts\activate

# Install
pip install -e .
```

---

### ❌ Error: `build dependencies failed`

**Problem:** Missing build tools

**Solution:**

**Mac:**
```bash
# Install Xcode command line tools
xcode-select --install
```

**Ubuntu/Debian:**
```bash
sudo apt install build-essential python3-dev
```

**Windows:**
- Install [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)

---

## Module Not Found Errors

### ❌ Error: `ModuleNotFoundError: No module named 'databricks_mcp'`

**Problem:** Python can't find the databricks_mcp module

**Solution 1:** Ensure package is installed
```bash
cd databricks-mcp
pip install -e .
```

**Solution 2:** Verify PYTHONPATH in mcp.json
```json
{
  "env": {
    "PYTHONPATH": "/absolute/path/to/databricks-mcp"
  }
}
```
- Must be the **absolute** path (full path, not relative)
- No trailing slashes
- Windows: Use forward slashes `C:/Users/...` or escaped backslashes `C:\\Users\\...`

**Solution 3:** Check which Python Cursor is using
```bash
# Find Python executable
# Mac/Linux:
which python
which python3

# Windows:
where python
```

Make sure this Python has databricks_mcp installed:
```bash
/path/to/python -c "import databricks_mcp; print('Found!')"
```

**Solution 4:** Specify full Python path in mcp.json
```json
{
  "command": "/full/path/to/python",
  "args": ["-m", "databricks_mcp.server.databricks_mcp_server"]
}
```

---

### ❌ Error: `Error while finding module specification`

**Problem:** Python module can't be found or executed

**Checklist:**
1. ✅ PYTHONPATH is set correctly in mcp.json
2. ✅ Path is absolute (not relative)
3. ✅ Package is installed: `pip install -e .`
4. ✅ Module name is correct: `databricks_mcp.server.databricks_mcp_server`
5. ✅ No typos in mcp.json

**Debug Steps:**
```bash
# Navigate to project
cd databricks-mcp

# Test module import
python -c "import databricks_mcp"

# Test module execution
python -m databricks_mcp.server.databricks_mcp_server
```

If these work but Cursor doesn't, the issue is with mcp.json configuration.

---

## Authentication Issues

### ❌ Error: `Authentication failed` or `401 Unauthorized`

**Problem:** Invalid or expired Databricks credentials

**Solution:**

1. **Verify your token is correct:**
   - Go to Databricks
   - User Settings → Developer → Access Tokens
   - Check if your token is still active
   - Generate a new token if needed

2. **Check DATABRICKS_HOST format:**
   ```
   ✅ Correct: https://dbc-12345678-abcd.cloud.databricks.com
   ✅ Correct: https://yourcompany.cloud.databricks.com
   ❌ Wrong: dbc-12345678-abcd.cloud.databricks.com (missing https://)
   ❌ Wrong: https://dbc-12345678-abcd.cloud.databricks.com/ (trailing slash)
   ```

3. **Update mcp.json with new credentials:**
   ```json
   {
     "env": {
       "DATABRICKS_HOST": "https://your-workspace.databricks.com",
       "DATABRICKS_TOKEN": "dapiNEW_TOKEN_HERE"
     }
   }
   ```

4. **Restart Cursor** after updating credentials

---

### ❌ Error: `Token has expired`

**Problem:** Personal access token has reached its expiration date

**Solution:**

1. Generate a new token in Databricks:
   - User Settings → Developer → Access Tokens
   - "Generate New Token"
   - Set appropriate lifetime

2. Update your token in mcp.json

3. Restart Cursor

---

## Cursor Integration Issues

### ❌ Tools not showing in Cursor

**Problem:** Databricks MCP tools are not available in Cursor

**Solution:**

1. **Check mcp.json syntax:**
   ```bash
   # Validate JSON syntax
   python -m json.tool ~/.cursor/mcp.json
   ```
   If this shows an error, fix the JSON syntax.

2. **Verify file location:**
   - **Mac/Linux:** `~/.cursor/mcp.json`
   - **Windows:** `C:\Users\YourName\.cursor\mcp.json`

3. **Completely restart Cursor:**
   - Not just reload window
   - Quit and reopen the application

4. **Check Cursor MCP logs:**
   - View → Output
   - Select "MCP" from dropdown
   - Look for databricks-mcp startup messages or errors

5. **Verify server name:**
   ```json
   {
     "mcpServers": {
       "databricks-mcp": {  // This name should match
         ...
       }
     }
   }
   ```

---

### ❌ Error: `MCP server failed to start`

**Problem:** Server crashes on startup

**Solution:**

1. **Test server directly:**
   ```bash
   cd databricks-mcp
   python -m databricks_mcp.server.databricks_mcp_server
   ```
   Look for error messages.

2. **Check environment variables:**
   - Verify DATABRICKS_HOST is set
   - Verify DATABRICKS_TOKEN is set
   - Check for typos

3. **Check logs:**
   - `databricks_mcp.log` in project directory
   - Cursor Output → MCP

4. **Try minimal configuration:**
   ```json
   {
     "mcpServers": {
       "databricks-mcp": {
         "command": "python",
         "args": ["-m", "databricks_mcp.server.databricks_mcp_server"],
         "env": {
           "DATABRICKS_HOST": "https://your-workspace.databricks.com",
           "DATABRICKS_TOKEN": "dapiXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
           "PYTHONPATH": "/absolute/path/to/databricks-mcp"
         }
       }
     }
   }
   ```

---

## Connection Issues

### ❌ Error: `Connection timeout` or `Connection refused`

**Problem:** Can't connect to Databricks workspace

**Solution:**

1. **Check network connection:**
   - Verify you can access Databricks in browser
   - Check if you're behind a firewall/proxy

2. **Verify workspace URL:**
   ```bash
   # Test connection
   curl -I https://your-workspace.databricks.com
   ```

3. **Check firewall settings:**
   - Ensure outbound HTTPS (port 443) is allowed
   - Check corporate proxy settings

4. **Try from different network:**
   - If on corporate VPN, try without
   - Or vice versa

---

### ❌ Error: `SSL certificate verification failed`

**Problem:** SSL/TLS certificate issues

**Solution:**

**Not Recommended (security risk):**
```python
# Only for testing, never in production
import os
os.environ['PYTHONHTTPSVERIFY'] = '0'
```

**Better Solution:**
```bash
# Update certificates
# Mac:
/Applications/Python\ 3.10/Install\ Certificates.command

# Ubuntu:
sudo apt install ca-certificates
sudo update-ca-certificates
```

---

## Tool Execution Issues

### ❌ Error: `Missing required parameter`

**Problem:** Tool called without required parameters

**Solution:**

Check the tool description for required parameters:
```
list_clusters - No parameters required
create_cluster - Requires: cluster_name, spark_version, node_type_id
get_cluster - Requires: cluster_id
```

In Cursor, provide parameters:
```
"Create a cluster named 'test-cluster' with spark_version '11.3.x-scala2.12' and node_type 'i3.xlarge'"
```

---

### ❌ Error: `Warehouse ID required`

**Problem:** SQL execution requires warehouse ID but none provided

**Solution 1:** Set default warehouse in mcp.json
```json
{
  "env": {
    "DATABRICKS_WAREHOUSE_ID": "sql_warehouse_xxxxx"
  }
}
```

**Solution 2:** Provide warehouse_id when calling execute_sql
```
"Execute SQL on warehouse sql_warehouse_12345: SELECT * FROM table"
```

---

### ❌ Error: `Resource not found (404)`

**Problem:** Trying to access non-existent cluster/job/notebook

**Solution:**

1. **List available resources:**
   ```
   "List my Databricks clusters"
   "List my Databricks jobs"
   "List notebooks in /Users/myname"
   ```

2. **Verify resource name/ID:**
   - Check spelling
   - Use exact cluster ID from list_clusters
   - Use full notebook path

---

## Logging and Debugging

### Enable Debug Logging

**Method 1:** Set in mcp.json
```json
{
  "env": {
    "LOG_LEVEL": "DEBUG"
  }
}
```

**Method 2:** Set in .env file
```env
LOG_LEVEL=DEBUG
```

### Check Server Logs

**Log File Location:**
```bash
# In your project directory
cat databricks_mcp.log

# Watch in real-time
tail -f databricks_mcp.log
```

### Check Cursor Logs

1. Open Cursor
2. View → Output
3. Select "MCP" from dropdown
4. Look for databricks-mcp entries

### Test Individual Components

**Test authentication:**
```bash
python -c "
from databricks_mcp.core.config import settings
print(f'Host: {settings.DATABRICKS_HOST}')
print(f'Token: {settings.DATABRICKS_TOKEN[:10]}...')
"
```

**Test API call:**
```bash
python -c "
import asyncio
from databricks_mcp.api import clusters
result = asyncio.run(clusters.list_clusters())
print(result)
"
```

---

## Common Configuration Mistakes

### ❌ Windows Path Issues

```json
// ❌ Wrong (single backslashes)
"PYTHONPATH": "C:\Users\Name\databricks-mcp"

// ✅ Correct (forward slashes)
"PYTHONPATH": "C:/Users/Name/databricks-mcp"

// ✅ Also correct (escaped backslashes)
"PYTHONPATH": "C:\\Users\\Name\\databricks-mcp"
```

### ❌ Trailing Commas in JSON

```json
// ❌ Wrong (trailing comma)
{
  "env": {
    "DATABRICKS_HOST": "https://...",
  }
}

// ✅ Correct (no trailing comma)
{
  "env": {
    "DATABRICKS_HOST": "https://..."
  }
}
```

### ❌ Missing PYTHONPATH

```json
// ❌ Incomplete (will fail with ModuleNotFoundError)
{
  "command": "python",
  "env": {
    "DATABRICKS_HOST": "https://..."
  }
}

// ✅ Complete (includes PYTHONPATH)
{
  "command": "python",
  "env": {
    "DATABRICKS_HOST": "https://...",
    "PYTHONPATH": "/path/to/databricks-mcp"
  }
}
```

---

## Still Having Issues?

### Collect Debug Information

```bash
# System info
python --version
pip --version
which python

# Installation check
cd databricks-mcp
pip show databricks-mcp-server

# Module import test
python -c "import databricks_mcp; print(databricks_mcp.__file__)"

# Server test
python -m databricks_mcp.server.databricks_mcp_server
```

### Get Help

1. **Check existing issues:** [GitHub Issues](https://github.com/THEAVINASHREDDY/databricks-mcp/issues)
2. **Read documentation:** [README.md](README.md) | [INSTALLATION.md](INSTALLATION.md)
3. **Open new issue:** Provide:
   - Operating system
   - Python version
   - Error messages
   - Steps to reproduce
   - Your mcp.json (remove sensitive data!)

---

**Quick Links:**
- [Quick Start](QUICKSTART.md)
- [Full Installation Guide](INSTALLATION.md)
- [Main Documentation](README.md)
- [Bug Fixes](FIXES_SUMMARY.md)

