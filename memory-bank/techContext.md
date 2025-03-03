# Technical Context

## Technology Stack Overview
```mermaid
flowchart TD
    subgraph Frontend
        Vue[Vue.js 3]
        TS[TypeScript]
        Vite
        Tailwind[TailwindCSS]
        Pinia
        Vitest
    end

    subgraph Backend
        Python[Python 3.11+]
        UV[uv package manager]
        Pytest
        Black
        Ruff
    end

    subgraph Tools
        VSCode[VS Code]
        Git
        Hooks[Pre-commit hooks]
    end

    Frontend --> Tools
    Backend --> Tools
```

## Current Technology Stack

### Frontend (Vue.js)
- Vue.js 3 with TypeScript
- Vite for build tooling
- TailwindCSS for styling
- Pinia for state management
- Vitest for testing

### Backend (Python)
- Python 3.11+
- uv for package management
- pytest for testing
- Black/isort for formatting
- Ruff for linting

### Documentation
- Markdown format
- markdownlint for style enforcement

## Development Workflow
```mermaid
flowchart LR
    subgraph Local
        Code[Code Changes] --> Hooks[Pre-commit Hooks]
        Hooks --> Tests[Run Tests]
    end

    subgraph Quality
        Format[Format Code]
        Lint[Lint Code]
        TypeCheck[Type Check]
    end

    Hooks --> Quality
    Tests --> Commit[Git Commit]

    subgraph Coverage
        Test[Test Coverage >= 80%]
    end

    Tests --> Coverage
```

## Development Tools
- VSCode as primary IDE
- Pre-commit hooks for code quality
- Version control with Git

## Configuration Standards
- Use of pyproject.toml for Python projects
- Tailwind config for frontend styling
- Consistent formatting rules across projects

## Quality Assurance
- Minimum 80% test coverage requirement
- Automated linting and formatting
- Type safety enforcement
- Regular dependency updates
