## Features

- **MCP Protocol Support**: Implements the MCP protocol to allow LLMs to interact with Databricks
- **Databricks API Integration**: Provides access to Databricks REST API functionality
- **Tool Registration**: Exposes Databricks functionality as MCP tools
- **Async Support**: Built with asyncio for efficient operation

## Available Tools

The Databricks MCP Server exposes the following tools:

### Cluster Management
- **list_clusters**: List all Databricks clusters
- **create_cluster**: Create a new Databricks cluster
- **terminate_cluster**: Terminate a Databricks cluster
- **get_cluster**: Get information about a specific Databricks cluster
- **start_cluster**: Start a terminated Databricks cluster

### Job Management
- **list_jobs**: List all Databricks jobs
- **run_job**: Run a Databricks job
- **run_notebook**: Submit and wait for a one-time notebook run
- **create_job**: Create a new Databricks job
- **delete_job**: Delete a Databricks job
- **get_run_status**: Get status information for a job run
- **list_job_runs**: List recent runs for a job
- **cancel_run**: Cancel a running job

### Workspace Files
- **list_notebooks**: List notebooks in a workspace directory
- **export_notebook**: Export a notebook from the workspace
- **import_notebook**: Import a notebook into the workspace
- **delete_workspace_object**: Delete a notebook or directory
- **get_workspace_file_content**: Retrieve content of any workspace file (JSON, notebooks, scripts, etc.)
- **get_workspace_file_info**: Get metadata about workspace files

### File System
- **list_files**: List files and directories in a DBFS path
- **dbfs_put**: Upload a small file to DBFS
- **dbfs_delete**: Delete a DBFS file or directory

### Cluster Libraries
- **install_library**: Install libraries on a cluster
- **uninstall_library**: Remove libraries from a cluster
- **list_cluster_libraries**: Check installed libraries on a cluster

### Repos
- **create_repo**: Clone a Git repository
- **update_repo**: Update an existing repo
- **list_repos**: List repos in the workspace
- **pull_repo**: Pull the latest commit for a Databricks repo

### Unity Catalog
- **list_catalogs**: List catalogs
- **create_catalog**: Create a catalog
- **list_schemas**: List schemas in a catalog
- **create_schema**: Create a schema
- **list_tables**: List tables in a schema
- **create_table**: Execute a CREATE TABLE statement
- **get_table_lineage**: Fetch lineage information for a table

### Composite
- **sync_repo_and_run_notebook**: Pull a repo and execute a notebook in one call

### SQL Execution
- **execute_sql**: Execute a SQL statement (warehouse_id optional if DATABRICKS_WAREHOUSE_ID env var is set)

## 🎉 Recent Updates (v0.3.0)

**New Features - Repo Sync & Notebook Execution:**
- ✅ **Repository Management**: Pull latest commits from Databricks repos with `pull_repo` tool
- ✅ **One-time Notebook Execution**: Submit and wait for notebook runs with `run_notebook` tool  
- ✅ **Composite Operations**: Combined repo sync + notebook execution with `sync_repo_and_run_notebook` tool
- ✅ **Enhanced Job Management**: Extended job APIs with submit, status checking, and run management
- ✅ **Comprehensive Testing**: Full test coverage for all new functionality

**Bug Fixes:**
- ✅ **Issue #9 Fixed**: Resolved "Missing required parameter 'params'" error in Cursor and other MCP clients
- ✅ **Parameter Handling**: All MCP tools now correctly handle both nested and flat parameter structures
- ✅ **Cursor Compatibility**: Full compatibility with Cursor's MCP implementation

**Previous Updates:**
- **v0.2.1**: Enhanced Codespaces support, documentation improvements, publishing process streamlining
- **v0.2.0**: Major package refactoring from `src/` to `databricks_mcp/` structure

**Backwards Compatibility:** All existing MCP tools continue to work unchanged. New features extend functionality without breaking changes.

## Installation
You'll need to set these environment variables:

- `DATABRICKS_HOST` - Your Databricks workspace URL
- `DATABRICKS_TOKEN` - Your Databricks personal access token  
- `DATABRICKS_WAREHOUSE_ID` - (Optional) Your default SQL warehouse ID

### 🔧 Local Installation (Development & Latest Features)

**Want to use the latest bug fixes and features?** Install from source for development or to get the most recent improvements.

👉 **[Quick Start Guide →](QUICKSTART.md)** | **[Detailed Instructions →](INSTALLATION.md)**

#### Quick Local Setup

```bash
# 1. Clone the repository
git clone https://github.com/THEAVINASHREDDY/databricks-mcp.git
cd databricks-mcp

# 2. Install in editable mode
pip install -e .

# 3. Configure mcp.json with your credentials + PYTHONPATH
```

**Benefits of Local Installation:**
- ✅ Get latest bug fixes immediately (see [FIXES_SUMMARY.md](FIXES_SUMMARY.md))
- ✅ Code changes take effect instantly (editable install)
- ✅ Full control over the installation
- ✅ Easy to contribute improvements back

**Configuration for Local Install:**

Update your `~/.cursor/mcp.json`:

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
        "PYTHONPATH": "/absolute/path/to/databricks-mcp"
      }
    }
  }
}
```

**Documentation:**
- 🚀 [QUICKSTART.md](QUICKSTART.md) - Get started in 5 minutes
- 📦 [INSTALLATION.md](INSTALLATION.md) - Detailed platform-specific instructions
- 🔧 [TROUBLESHOOTING.md](TROUBLESHOOTING.md) - Common issues and solutions
- 🐛 [FIXES_SUMMARY.md](FIXES_SUMMARY.md) - Latest bug fixes and improvements

### Manual Installation

#### Prerequisites

- Python 3.10 or higher
- `uv` package manager (recommended for MCP servers)

### Setup

1. Install `uv` if you don't have it already:

   ```bash
   # MacOS/Linux
   curl -LsSf https://astral.sh/uv/install.sh | sh
   
   # Windows (in PowerShell)
   irm https://astral.sh/uv/install.ps1 | iex
   ```

   Restart your terminal after installation.

2. Clone the repository:
   ```bash
   git clone https://github.com/markov-kernel/databricks-mcp.git
   cd databricks-mcp
   ```

3. Run the setup script:
   ```bash
   # Linux/Mac
   ./scripts/setup.sh
   
   # Windows (PowerShell)
   .\scripts\setup.ps1
   ```

   The setup script will:
   - Install `uv` if not already installed
   - Create a virtual environment
   - Install all project dependencies
   - Verify the installation works

   **Alternative manual setup:**
   ```bash
   # Create and activate virtual environment
   uv venv
   
   # On Windows
   .\.venv\Scripts\activate
   
   # On Linux/Mac
   source .venv/bin/activate
   
   # Install dependencies in development mode
   uv pip install -e .
   
   # Install development dependencies
   uv pip install -e ".[dev]"
   ```

4. Set up environment variables:
   ```bash
   # Required variables
   # Windows
   set DATABRICKS_HOST=https://your-databricks-instance.azuredatabricks.net
   set DATABRICKS_TOKEN=your-personal-access-token
   
   # Linux/Mac
   export DATABRICKS_HOST=https://your-databricks-instance.azuredatabricks.net
   export DATABRICKS_TOKEN=your-personal-access-token
   
   # Optional: Set default SQL warehouse (makes warehouse_id optional in execute_sql)
   export DATABRICKS_WAREHOUSE_ID=sql_warehouse_12345
   ```

   You can also create an `.env` file based on the `.env.example` template.

## Running the MCP Server

### Standalone

To start the MCP server directly for testing or development, run:

```bash
# Activate your virtual environment if not already active
source .venv/bin/activate 

# Run the start script (handles finding env vars from .env if needed)
./scripts/start_mcp_server.sh
```

This is useful for seeing direct output and logs.

### Integrating with AI Clients

To use this server with AI clients like Cursor or Claude CLI, you need to register it.

#### Cursor Setup

1.  Open your global MCP configuration file located at `~/.cursor/mcp.json` (create it if it doesn't exist).
2.  Add the following entry within the `mcpServers` object, replacing placeholders with your actual values and ensuring the path to `start_mcp_server.sh` is correct:

    ```json
    {
      "mcpServers": {
        // ... other servers ...
        "databricks-mcp-local": { 
          "command": "/absolute/path/to/your/project/databricks-mcp-server/start_mcp_server.sh",
          "args": [],
          "env": {
            "DATABRICKS_HOST": "https://your-databricks-instance.azuredatabricks.net", 
            "DATABRICKS_TOKEN": "dapiXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
            "DATABRICKS_WAREHOUSE_ID": "sql_warehouse_12345",
            "RUNNING_VIA_CURSOR_MCP": "true" 
          }
        }
        // ... other servers ...
      }
    }
    ```

3.  **Important:** Replace `/absolute/path/to/your/project/databricks-mcp-server/` with the actual absolute path to this project directory on your machine.
4.  Replace the `DATABRICKS_HOST` and `DATABRICKS_TOKEN` values with your credentials.
5.  Save the file and **restart Cursor**.

6.  You can now invoke tools using `databricks-mcp-local:<tool_name>` (e.g., `databricks-mcp-local:list_jobs`).

#### Claude CLI Setup

1.  Use the `claude mcp add` command to register the server. Provide your credentials using the `-e` flag for environment variables and point the command to the `start_mcp_server.sh` script using `--` followed by the absolute path:

    ```bash
    claude mcp add databricks-mcp-local \
      -s user \
      -e DATABRICKS_HOST="https://your-databricks-instance.azuredatabricks.net" \
      -e DATABRICKS_TOKEN="dapiXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" \
      -e DATABRICKS_WAREHOUSE_ID="sql_warehouse_12345" \
      -- /absolute/path/to/your/project/databricks-mcp-server/start_mcp_server.sh
    ```

2.  **Important:** Replace `/absolute/path/to/your/project/databricks-mcp-server/` with the actual absolute path to this project directory on your machine.
3.  Replace the `DATABRICKS_HOST` and `DATABRICKS_TOKEN` values with your credentials.

4.  You can now invoke tools using `databricks-mcp-local:<tool_name>` in your Claude interactions.

## Querying Databricks Resources

The repository includes utility scripts to quickly view Databricks resources:

```bash
# View all clusters
uv run scripts/show_clusters.py

# View all notebooks
uv run scripts/show_notebooks.py
```

## Usage Examples

### SQL Execution with Default Warehouse
```python
# With DATABRICKS_WAREHOUSE_ID set, warehouse_id is optional
await session.call_tool("execute_sql", {
    "statement": "SELECT * FROM my_table LIMIT 10"
})

# You can still override the default warehouse
await session.call_tool("execute_sql", {
    "statement": "SELECT * FROM my_table LIMIT 10",
    "warehouse_id": "sql_warehouse_specific"
})
```

### Workspace File Content Retrieval
```python
# Get JSON file content from workspace
await session.call_tool("get_workspace_file_content", {
    "workspace_path": "/Users/user@domain.com/config/settings.json"
})

# Get notebook content in Jupyter format
await session.call_tool("get_workspace_file_content", {
    "workspace_path": "/Users/user@domain.com/my_notebook",
    "format": "JUPYTER"
})

# Get file metadata without downloading content
await session.call_tool("get_workspace_file_info", {
    "workspace_path": "/Users/user@domain.com/large_file.py"
})
```

### Repo Sync and Notebook Execution
```python
await session.call_tool("sync_repo_and_run_notebook", {
    "repo_id": 123,
    "notebook_path": "/Repos/user/project/run_me"
})
```

### Create Nightly ETL Job
```python
job_conf = {
    "name": "Nightly ETL",
    "tasks": [
        {
            "task_key": "etl",
            "notebook_task": {"notebook_path": "/Repos/me/etl.py"},
            "existing_cluster_id": "abc-123"
        }
    ]
}
await session.call_tool("create_job", job_conf)
```

## Project Structure

```
databricks-mcp/
├── databricks_mcp/                  # Main package (renamed from src/)
│   ├── __init__.py                  # Package initialization
│   ├── __main__.py                  # Main entry point for the package
│   ├── main.py                      # Entry point for the MCP server
│   ├── api/                         # Databricks API clients
│   │   ├── clusters.py              # Cluster management
│   │   ├── jobs.py                  # Job management
│   │   ├── notebooks.py             # Notebook operations
│   │   ├── sql.py                   # SQL execution
│   │   └── dbfs.py                  # DBFS operations
│   ├── core/                        # Core functionality
│   │   ├── config.py                # Configuration management
│   │   ├── auth.py                  # Authentication
│   │   └── utils.py                 # Utilities
│   ├── server/                      # Server implementation
│   │   ├── __main__.py              # Server entry point
│   │   ├── databricks_mcp_server.py # Main MCP server
│   │   └── app.py                   # FastAPI app for tests
│   └── cli/                         # Command-line interface
│       └── commands.py              # CLI commands
├── tests/                           # Test directory
│   ├── test_clusters.py             # Cluster tests
│   ├── test_mcp_server.py           # Server tests
│   └── test_*.py                    # Other test files
├── scripts/                         # Helper scripts (organized)
│   ├── start_mcp_server.ps1         # Server startup script (Windows)
│   ├── start_mcp_server.sh          # Server startup script (Unix)
│   ├── run_tests.ps1                # Test runner script (Windows)
│   ├── run_tests.sh                 # Test runner script (Unix)
│   ├── setup.ps1                    # Setup script (Windows)
│   ├── setup.sh                     # Setup script (Unix)
│   ├── show_clusters.py             # Script to show clusters
│   ├── show_notebooks.py            # Script to show notebooks
│   ├── setup_codespaces.sh          # Codespaces setup
│   └── test_setup_local.sh          # Local test setup
├── examples/                        # Example usage
│   ├── direct_usage.py              # Direct usage examples
│   └── mcp_client_usage.py          # MCP client examples
├── docs/                            # Documentation (organized)
│   ├── AGENTS.md                    # Agent documentation
│   ├── project_structure.md         # Detailed structure docs
│   ├── new_features.md              # Feature documentation
│   └── phase1.md                    # Development phases
├── .gitignore                       # Git ignore rules
├── .cursor.json                     # Cursor configuration
├── pyproject.toml                   # Package configuration
├── uv.lock                          # Dependency lock file
└── README.md                        # This file
```

See `docs/project_structure.md` for a more detailed view of the project structure.

## Development

### Code Standards

- Python code follows PEP 8 style guide with a maximum line length of 100 characters
- Use 4 spaces for indentation (no tabs)
- Use double quotes for strings
- All classes, methods, and functions should have Google-style docstrings
- Type hints are required for all code except tests

### Linting

The project uses the following linting tools:

```bash
# Run all linters
uv run pylint databricks_mcp/ tests/
uv run flake8 databricks_mcp/ tests/
uv run mypy databricks_mcp/
```

## Testing

The project uses pytest for testing. To run the tests:

```bash
# Run all tests with our convenient script
.\scripts\run_tests.ps1

# Run with coverage report
.\scripts\run_tests.ps1 -Coverage

# Run specific tests with verbose output
.\scripts\run_tests.ps1 -Verbose -Coverage tests/test_clusters.py
```

You can also run the tests directly with pytest:

```bash
# Run all tests
uv run pytest tests/

# Run with coverage report
uv run pytest --cov=databricks_mcp tests/ --cov-report=term-missing
```

A minimum code coverage of 80% is the goal for the project.

## Documentation

- API documentation is generated using Sphinx and can be found in the `docs/api` directory
- All code includes Google-style docstrings
- See the `examples/` directory for usage examples

## Examples

Check the `examples/` directory for usage examples. To run examples:

```bash
# Run example scripts with uv
uv run examples/direct_usage.py
uv run examples/mcp_client_usage.py
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Ensure your code follows the project's coding standards
2. Add tests for any new functionality
3. Update documentation as necessary
4. Verify all tests pass before submitting

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## About

 A Model Completion Protocol (MCP) server for interacting with Databricks services.

Credit for the initial version goes to [@JustTryAI](https://github.com/JustTryAI/databricks-mcp-server).
