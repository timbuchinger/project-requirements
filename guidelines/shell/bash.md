# Bash Scripting Guidelines

## Environment
- Use /bin/bash shebang
- Set strict mode: set -euo pipefail
- Exit on error: trap 'exit' ERR

## Best Practices
- Use shellcheck
- Functions for reusable code
- Validate input parameters
- Log meaningful messages

## Structure
- Main function pattern
- Source common utilities
- Version in header comment
- Usage documentation
