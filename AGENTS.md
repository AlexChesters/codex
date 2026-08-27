# ~/.codex/AGENTS.md

## Python
- Avoid creating very large Python files
  - If creating several custom exceptions prefer creating an `errors.py` to contain them
- Always `import` at the top level of a file; NEVER `import` packages inside a function body
- Use `uv` for dependency management
- Use `ruff` for linting
- Use `requests` for making HTTP requests
- Do not write docstrings/comments for clean code; reserve comments for unusual situations
