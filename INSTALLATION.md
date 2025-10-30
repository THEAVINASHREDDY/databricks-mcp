# 📦 Installation Guide

Complete installation instructions for setting up Databricks MCP Server locally.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Platform-Specific Instructions](#platform-specific-instructions)
- [Verification](#verification)
- [Updates](#updates)

---

## Prerequisites

### Required Software

#### Python 3.10 or Higher

**Check your Python version:**
```bash
python --version
# or
python3 --version
```

**Install Python if needed:**
- **Mac:** `brew install python@3.10` or download from [python.org](https://www.python.org/downloads/)
- **Linux:** `sudo apt install python3.10 python3-pip`
- **Windows:** Download from [python.org](https://www.python.org/downloads/)

#### Git

**Check if Git is installed:**
```bash
git --version
```

**Install Git if needed:**
- **Mac:** `brew install git` or download from [git-scm.com](https://git-scm.com/)
- **Linux:** `sudo apt install git`
- **Windows:** Download from [git-scm.com](https://git-scm.com/)

### Databricks Requirements

You'll need:

1. **Databricks Workspace URL**
   - Example: `https://dbc-12345678-abcd.cloud.databricks.com`
   - Found in your browser when you're logged into Databricks

2. **Personal Access Token**
   - Navigate to: Databricks → User Settings → Developer → Access Tokens
   - Click "Generate New Token"
   - Give it a name (e.g., "MCP Server")
   - Copy the token immediately (you won't see it again)

3. **SQL Warehouse ID** (Optional, for SQL operations)
   - Navigate to: SQL Warehouses
   - Click on your warehouse
   - Find the ID in "Connection Details"

---

## Installation

### Step 1: Clone the Repository

```bash
# Clone from GitHub
git clone https://github.com/THEAVINASHREDDY/databricks-mcp.git

# Navigate into the directory
cd databricks-mcp
```

### Step 2: Install the Package

We'll install in **editable mode** so code changes take effect immediately without reinstalling.

#### Using pip (Standard Method)

```bash
# Install with all dependencies
pip install -e .

# If you get permission errors, try:
pip install --user -e .
```

#### Using pip3 (If python3 is your command)

```bash
pip3 install -e .
```

#### Using uv (Optional - Faster)

```bash
# Install uv first
curl -LsSf https://astral.sh/uv/install.sh | sh  # Mac/Linux
# or
irm https://astral.sh/uv/install.ps1 | iex  # Windows PowerShell

# Install the package
uv pip install -e .
```

### Step 3: Verify Installation

```bash
# Check if the module can be imported
python -c "import databricks_mcp; print('Success!')"

# Try running the server (Ctrl+C to stop)
python -m databricks_mcp.server.databricks_mcp_server
```

If you see server startup logs, installation is successful!

---

## Configuration

### Option 1: Environment Variables (Recommended for Development)

Create a `.env` file in the project root:

```bash
# Copy the example file
cp .env.example .env

# Edit with your preferred editor
nano .env
# or
vim .env
# or open in your IDE
```

Fill in your credentials:

```env
DATABRICKS_HOST=https://your-workspace.databricks.com
DATABRICKS_TOKEN=dapiXXXXXXXXXXXXXXXXXXXXXXXXXXXX
DATABRICKS_WAREHOUSE_ID=sql_warehouse_xxxxx
LOG_LEVEL=INFO
```

**Security Note:** The `.env` file is already in `.gitignore` and will not be committed.

### Option 2: Direct Configuration in MCP Client

You can provide credentials directly in your MCP client configuration (see next section).

---

## MCP Client Configuration

### Cursor IDE

#### Step 1: Find Your Project Path

```bash
# Mac/Linux:
cd databricks-mcp
pwd

# Windows PowerShell:
cd databricks-mcp
Get-Location
```

Copy the full path (e.g., `/Users/yourname/databricks-mcp`)

#### Step 2: Edit MCP Configuration

**File Location:**
- **Mac/Linux:** `~/.cursor/mcp.json`
- **Windows:** `C:\Users\YourName\.cursor\mcp.json`

**Create or edit the file:**

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

**Important Replacements:**
- `DATABRICKS_HOST` → Your actual workspace URL
- `DATABRICKS_TOKEN` → Your actual access token
- `DATABRICKS_WAREHOUSE_ID` → Your actual warehouse ID (or remove if not using SQL)
- `PYTHONPATH` → The full path you copied in Step 1

#### Step 3: Restart Cursor

Close and reopen Cursor completely for changes to take effect.

---

## Platform-Specific Instructions

### 🍎 macOS

#### Using System Python

```bash
# Check Python version
python3 --version

# Install the package
cd databricks-mcp
pip3 install -e .

# Find Python path
which python3
```

#### Using Homebrew Python

```bash
# Install Python if needed
brew install python@3.10

# Install the package
cd databricks-mcp
pip3 install -e .
```

#### mcp.json Configuration

```json
{
  "mcpServers": {
    "databricks-mcp": {
      "command": "python3",
      "args": ["-m", "databricks_mcp.server.databricks_mcp_server"],
      "env": {
        "DATABRICKS_HOST": "https://your-workspace.databricks.com",
        "DATABRICKS_TOKEN": "dapiXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        "DATABRICKS_WAREHOUSE_ID": "sql_warehouse_xxxxx",
        "PYTHONPATH": "/Users/yourname/databricks-mcp"
      }
    }
  }
}
```

---

### 🐧 Linux (Ubuntu/Debian)

#### Install Prerequisites

```bash
# Update package list
sudo apt update

# Install Python and pip
sudo apt install python3.10 python3-pip git

# Verify installation
python3 --version
pip3 --version
```

#### Install Databricks MCP

```bash
# Clone repository
git clone https://github.com/THEAVINASHREDDY/databricks-mcp.git
cd databricks-mcp

# Install package
pip3 install -e .
```

#### mcp.json Configuration

```json
{
  "mcpServers": {
    "databricks-mcp": {
      "command": "python3",
      "args": ["-m", "databricks_mcp.server.databricks_mcp_server"],
      "env": {
        "DATABRICKS_HOST": "https://your-workspace.databricks.com",
        "DATABRICKS_TOKEN": "dapiXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        "DATABRICKS_WAREHOUSE_ID": "sql_warehouse_xxxxx",
        "PYTHONPATH": "/home/yourname/databricks-mcp"
      }
    }
  }
}
```

---

### 🪟 Windows

#### Install Prerequisites

1. **Install Python:**
   - Download from [python.org](https://www.python.org/downloads/)
   - ✅ Check "Add Python to PATH" during installation
   - Verify: `python --version`

2. **Install Git:**
   - Download from [git-scm.com](https://git-scm.com/)
   - Use default installation options

#### Install Databricks MCP

```powershell
# Open PowerShell

# Clone repository
git clone https://github.com/THEAVINASHREDDY/databricks-mcp.git
cd databricks-mcp

# Install package
pip install -e .
```

#### mcp.json Configuration

**File Location:** `C:\Users\YourName\.cursor\mcp.json`

**Important:** Use forward slashes or escaped backslashes in paths:

```json
{
  "mcpServers": {
    "databricks-mcp": {
      "command": "python",
      "args": ["-m", "databricks_mcp.server.databricks_mcp_server"],
      "env": {
        "DATABRICKS_HOST": "https://your-workspace.databricks.com",
        "DATABRICKS_TOKEN": "dapiXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        "DATABRICKS_WAREHOUSE_ID": "sql_warehouse_xxxxx",
        "PYTHONPATH": "C:/Users/YourName/databricks-mcp"
      }
    }
  }
}
```

**Path Format Options:**
- ✅ Forward slashes: `C:/Users/YourName/databricks-mcp`
- ✅ Escaped backslashes: `C:\\Users\\YourName\\databricks-mcp`
- ❌ Single backslashes: `C:\Users\YourName\databricks-mcp` (will cause errors)

---

## Verification

### Test Python Module

```bash
# Navigate to project directory
cd databricks-mcp

# Test import
python -c "import databricks_mcp; print('Module imported successfully!')"

# Test server startup (Ctrl+C to stop)
python -m databricks_mcp.server.databricks_mcp_server
```

### Test in Cursor

1. **Restart Cursor completely**
2. Open Cursor chat
3. Type: "List my Databricks clusters"
4. You should see your clusters listed

### Check Logs

**Cursor Logs:**
- View → Output → Select "MCP" from dropdown
- Look for databricks-mcp startup messages

**Server Logs:**
- Check `databricks_mcp.log` in your project directory

---

## Updates

### Pulling New Changes

When updates are available:

```bash
# Navigate to project directory
cd databricks-mcp

# Pull latest changes
git pull origin master

# No reinstall needed with editable install!
# Just restart Cursor to use new code
```

### Reinstalling (If Needed)

```bash
cd databricks-mcp
pip install -e . --force-reinstall
```

---

## Next Steps

- ✅ **Test the installation:** [See Verification section](#verification)
- ✅ **Learn available tools:** See [README.md](README.md#available-tools)
- ✅ **Troubleshoot issues:** See [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- ✅ **Review bug fixes:** See [FIXES_SUMMARY.md](FIXES_SUMMARY.md)

---

**Need help?** [Open an issue](https://github.com/THEAVINASHREDDY/databricks-mcp/issues) on GitHub!

