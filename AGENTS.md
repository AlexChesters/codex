# ~/.codex/AGENTS.md

## General advice
- Test observable behavior, not implementation details
  - Avoid “change-detector” tests that mechanically mirror code or assert incidental internal calls/order
  - Keep interaction assertions only when the interaction is part of the behavior contract
- Do not preserve backwards compatibility unless explicitly instructed to
- Prefer monorepos with the following structure:
  - `apps/` - isolated microservices
  - `common-resources/` - shared dependencies used by two or more apps, e.g. databases
  - `clients/` - client applications
    - `web/`
    - `ios/`
    - `android/`

## AWS
- Prefer `eu-west-1` unless another region is specified by the user or required due to service availability

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
- Prefer short form for CloudFormation functions where possible (`!Sub` instead of `!Fn::Sub`)
