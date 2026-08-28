# ~/.codex/AGENTS.md

## Python
- Avoid creating very large Python files
  - If creating several custom exceptions prefer creating an `errors.py` to contain them
- Avoid bloated, single-folder structures
  - Prefer `models/models.py`, `errors/errors.py`, `services/my_service.py` over `my_app/models.py`, `my_app/errors.py`, `my_app/my_service.py`
- Always `import` at the top level of a file; NEVER `import` packages inside a function body
- Use `uv` for dependency management
- Use `ruff` for linting
- Use `requests` for making HTTP requests
- Do not write docstrings/comments for clean code; reserve comments for unusual situations

## CloudFormation
- Use YAML for CloudFormation templates with a `.yml` file extension
- Do not include blank lines between resources
