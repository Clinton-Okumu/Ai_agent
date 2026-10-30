# Autonomous AI Coding Agent

An iterative, tool-calling AI coding agent built in Python using OpenRouter (OpenAI-compatible SDK). The agent can autonomously inspect file structures, read code files, make edits, and run Python scripts and test suites within a secured working directory to investigate and fix bugs.

Built as part of the **Build an AI Agent in Python** course on [Boot.dev](https://www.boot.dev).

---

## Features

- **Agentic Feedback Loop**: Executes up to 20 iterative turns, maintaining complete conversation and tool execution history until the task is resolved.
- **Strict Sandbox / Directory Scoping**: All tool actions are validated using path traversal checks (`os.path.commonpath`) to strictly confine operations within a target working directory (e.g. `./calculator`).
- **Function Calling & Declarations**: Uses JSON Schemas to declare and dispatch tool calls dynamically to the LLM.
- **Safety Truncation**: Safeguards LLM token consumption by capping file reads at 10,000 characters and appending a truncation indicator when files exceed the limit.
- **Subprocess Execution**: Safely executes Python files in a separate subprocess with a 30-second timeout guard.

---

## Agent Tools

| Tool | Description |
| :--- | :--- |
| `get_files_info` | Lists files and directories with sizes and directory flags. |
| `get_file_content` | Reads and returns file contents (truncated at `MAX_CHARS`). |
| `write_file` | Writes or overwrites files, automatically creating missing parent directories. |
| `run_python_file` | Executes Python scripts via subprocess and captures stdout, stderr, and exit codes. |

---

## Project Structure

```text
ai_agent/
├── calculator/               # Target working directory & sample project
│   ├── pkg/
│   │   ├── calculator.py     # Infix expression evaluator
│   │   └── render.py         # JSON rendering logic
│   ├── main.py               # Calculator CLI
│   └── tests.py              # Test suite for calculator logic
├── functions/                # Tool implementations and JSON schemas
│   ├── get_file_content.py
│   ├── get_files_info.py
│   ├── run_python_file.py
│   └── write_file.py
├── call_function.py          # Function map and dispatch logic
├── config.py                 # Project constants (e.g., MAX_CHARS)
├── prompts.py                # System instructions for the agent
├── main.py                   # Main CLI entry point & agent loop
├── pyproject.toml            # Project dependencies and config
└── test_*.py                 # Unit tests for agent tools
```

---

## Installation & Setup

### Prerequisites

- Python 3.13+
- [`uv`](https://github.com/astral-sh/uv) (recommended) or `pip`

### 1. Clone the repository

```bash
git clone git@github.com:Clinton-Okumu/Ai_agent.git
cd Ai_agent
```

### 2. Environment Variables

Create a `.env` file in the project root:

```bash
OPENROUTER_API_KEY="your-openrouter-api-key"
```

### 3. Install Dependencies

Using `uv`:

```bash
uv sync
```

---

## Usage

### Run a prompt with the agent

```bash
uv run main.py "Explain how the calculator renders the result to the console."
```

### Enable verbose logging

To see token usage metadata and intermediate tool call results:

```bash
uv run main.py --verbose "Fix the bug: 3 + 7 * 2 shouldn't be 20."
```

### Run Tool Tests

```bash
uv run test_get_files_info.py
uv run test_get_file_content.py
uv run test_write_file.py
uv run test_run_python_file.py
```

---

## License

MIT License
