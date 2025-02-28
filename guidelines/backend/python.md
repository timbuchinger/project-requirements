# Python Development Guidelines

## Environment
- Python 3.11+ required
- Use venv for virtual environments
  ```bash
  python -m venv .venv
  source .venv/bin/activate  # Linux/macOS
  .venv\Scripts\activate     # Windows
  ```
- uv as package manager (preferred over poetry)
  ```bash
  curl -LsSf https://astral.sh/uv/install.sh | sh
  uv venv .venv
  uv pip install <package>
  ```

## Code Quality
- Black for code formatting
  - Line length: 88 characters (default)
  - Use `pyproject.toml` for configuration
- isort for import sorting
  - Compatible with Black
  - Configure in `pyproject.toml`
- Ruff for linting
  - Replaces flake8, pylint
  - Fast and comprehensive
- Type hints mandatory
- mypy for static type checking

## Testing
- pytest as testing framework
  - Fixtures over setup/teardown
  - Parametrize tests when testing multiple cases
  - Use markers to categorize tests
- pytest-cov for coverage reporting
  - Minimum 80% test coverage
  - Coverage reports in HTML format
- pytest.ini or pyproject.toml configuration
- Mock external dependencies

## Project Structure
```
project_name/
├── src/
│   └── package_name/
│       ├── __init__.py
│       ├── module1.py
│       └── module2.py
├── tests/
│   ├── __init__.py
│   ├── test_module1.py
│   └── test_module2.py
├── .gitignore
├── README.md
├── pyproject.toml
└── requirements/
    ├── requirements.txt
    └── requirements-dev.txt
```

- src-layout pattern
- Modules over packages for simple projects
- Clear separation of concerns
- Keep modules focused and small

## Dependencies
- Requirements split into prod/dev
- Periodic dependency updates
- Pinned versions in requirements
- Use `pyproject.toml` for project metadata
- Example pyproject.toml:
  ```toml
  [project]
  name = "project_name"
  version = "0.1.0"
  description = "Project description"
  requires-python = ">=3.11"

  [build-system]
  requires = ["hatchling"]
  build-backend = "hatchling.build"

  [tool.black]
  line-length = 88

  [tool.isort]
  profile = "black"
  multi_line_output = 3

  [tool.ruff]
  line-length = 88
  target-version = "py311"
  ```

## Best Practices
- Docstring requirements (Google style)
  ```python
  def function(param1: str, param2: int) -> bool:
      """Short description.

      Longer description if needed.

      Args:
          param1: Parameter description.
          param2: Parameter description.

      Returns:
          Description of return value.

      Raises:
          ValueError: Description of when this error occurs.
      """
      pass
  ```
- Error handling patterns
  - Use specific exceptions
  - Context managers for cleanup
  - Never catch bare except
- FastAPI preferred for web services
  - Modern, fast, async by default
  - Type hints and automatic OpenAPI docs
  - Prefer over Flask or other frameworks
- Async/await when appropriate
  - Use asyncio for I/O-bound operations
- Context managers for resource management
  ```python
  from contextlib import contextmanager

  @contextmanager
  def managed_resource():
      try:
          # Setup
          yield
      finally:
          # Cleanup
          pass
  ```
- Dataclasses for data containers
  ```python
  from dataclasses import dataclass
  from datetime import datetime

  @dataclass
  class User:
      id: int
      name: str
      created_at: datetime
  ```
- Type annotations throughout
  ```python
  from typing import List, Optional

  def process_items(items: List[str], max_count: Optional[int] = None) -> List[str]:
      pass
  ```
- Use pathlib over os.path
- f-strings over .format() or %
- Use walrus operator (:=) when appropriate
- List comprehensions for simple transformations

## Development Tools
- VSCode with Python extension
- Setup pre-commit hooks:
  ```yaml
  # .pre-commit-config.yaml
  repos:
    - repo: https://github.com/astral-sh/ruff-pre-commit
      rev: v0.2.0
      hooks:
        - id: ruff
        - id: ruff-format
    - repo: https://github.com/pre-commit/mirrors-mypy
      rev: v1.8.0
      hooks:
        - id: mypy
    - repo: https://github.com/pycqa/isort
      rev: 5.13.2
      hooks:
        - id: isort
  ```

## Security
- Keep dependencies updated
- Use secrets management
- Input validation
- Use proper HTTPS/SSL
- Hash passwords with bcrypt
