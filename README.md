# Expense Tracker -- Local MCP Server

A simple **local MCP (Model Context Protocol) server** for tracking personal expenses using **SQLite**.
This server exposes expense-related tools (add, list, summarize) that can be consumed by MCP-compatible clients such as **Claude Desktop**, **Cursor**, or other LLM hosts.

No HTTP server.
No frontend.
Just a clean local tool backend for LLMs.

---

## Features

- **Add expense entries** with date, amount, category, subcategory, and notes
- **List expenses** within a date range
- **Summarize expenses** by category
- **Local SQLite database** (auto-created)
- **Categories exposed** as an MCP resource (`expense://categories`)
- **Zero configuration** beyond Python

---

## Project Structure

```
expense_tracker/
│
├── server.py          # MCP server code
├── categories.json    # Expense categories (editable at runtime)
└── expenses.db        # SQLite database (auto-created)
```

---

### Requirements

---

- Python **3.9+**

- [`uv`](https://github.com/astral-sh/uv) (recommended)

- `fastmcp`

---

### Setup

---

### 1\. Install `uv` (once)

```
pip install uv

```

Verify installation:

```
uv --version

```

### 2\. Create a virtual environment

From the project directory:

```
uv venv

```

**Activate it:**

- **Windows:** `.venv\Scripts\activate`

- **macOS / Linux:** `source .venv/bin/activate`

### 3\. Install dependencies

```
uv pip install fastmcp

```

---

### Running the MCP Server

---

Start the server using:

```
uv run python server.py

```

_Note: You will see no output. This is expected behavior as the server runs silently and waits for a compatible client (like Claude) to connect._

To stop the server: `CTRL + C`

### Database Behavior

- The `expenses.db` file is automatically created on first run.

- Tables are created using `CREATE TABLE IF NOT EXISTS`.

- No manual database setup is required.

---

### Available MCP Tools

---

#### `add_expense`

Adds a new expense entry to the database.

- **date**: string (format: `YYYY-MM-DD`)

- **amount**: number

- **category**: string

- **subcategory**: string (optional)

- **note**: string (optional)

#### `list_expenses`

Lists all expenses within a given date range.

- **start_date**: string (`YYYY-MM-DD`)

- **end_date**: string (`YYYY-MM-DD`)

#### `summarize`

Summarizes expenses by category.

- **start_date**: string (`YYYY-MM-DD`)

- **end_date**: string (`YYYY-MM-DD`)

- **category**: string (optional)

---

### MCP Resources

---

#### `expense://categories`

Returns the contents of `categories.json`. The file is read fresh on every request, allowing category updates without restarting the server.

---

### Using with Claude Desktop

---

Add the server to your `claude_desktop_config.json` (found in `%APPDATA%\Claude` on Windows or `~/Library/Application Support/Claude` on macOS):

```
{
  "mcpServers": {
    "expense-tracker": {
      "command": "uv",
      "args": [
        "run",
        "--path",
        "C:/absolute/path/to/expense_tracker",
        "python",
        "server.py"
      ]
    }
  }
}

```

_Note: Ensure you use absolute paths and forward slashes `/` even on Windows._

---

### License

---

MIT License --- free to use, modify, and distribute.
