# Piqo — Pi Extension

A chat-less way to communicate with your favorite LLM models (online or offline), directly from your files, that is triggered on file save. It is a simple file-watcher extension for [pi](https://github.com/badlogic/pi-mono) that monitors directories for `@piqo` markers and uses the LLM to generate content inline.

## How It Works

1. You start pi with the piqo extension and specify directories to watch
2. Piqo recursively watches those directories for file changes
3. When a file contains one or more `@piqo <instruction>` markers, it reads the file, gathers context around all markers, and sends them to pi's LLM in one request
4. The LLM fulfills each prompt and removes the human prompt line/tag from the file

## Usage

```bash
# Load it directly from github
pi -e https://github.com/piqoni/piqo-extension --dir=/path/to/your/project

# Or if you want to reference it locally, git clone the repo and reference it directly
pi -e ./piqo-extension --dir /path/to/your/project

# Watch multiple directories
pi -e ./piqo-extension --dir /path/to/dir1,/path/to/dir2

# Headless mode (no TUI)
pi -e ./piqo-extension --dir /path/to/project -p "Start piqo watcher"
```

## Marker Format

In any text file within the watched directories, add:

```
@piqo <your instruction here>
```

The LLM will process it and replace/remove the prompt so the file becomes:

```
... generated content ...
```

### Examples

**In a Python file:**
```python
# @piqo add a function to parse CSV files and return a list of dicts

# Becomes generated code with the @piqo prompt removed
```

**In a Markdown file:**
```markdown
@piqo write a summary of REST API best practices

Becomes generated content with the @piqo prompt removed
```

**In a config file:**
```yaml
# @piqo add sensible default nginx config for a Node.js app

# Becomes generated config with the @piqo prompt removed
```

## Behavior Details

- **Debounce**: File changes are debounced at 500ms per file to avoid duplicate processing
- **Initial scan**: On startup, piqo scans all watched directories for existing markers
- **Ignored paths**: Hidden files/dirs, `node_modules`, `.git` are automatically skipped
- **Text files only**: Only processes common text file extensions (.ts, .js, .py, .md, .txt, etc.)

## Installation

Place this extension in `~/.pi/agent/extensions/piqo/` for global access, or reference it directly:

```bash
pi -e /path/to/piqo-extension
```
