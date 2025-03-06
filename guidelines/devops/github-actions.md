# GitHub Actions Guidelines

## Workflow Structure
```
.github/
├── workflows/           # Workflow definitions
│   ├── ci.yml
│   ├── cd.yml
│   └── reusable/       # Reusable workflow components
├── actions/            # Custom actions
└── scripts/           # Supporting scripts

workflows/reusable/
├── build.yml          # Reusable build workflow
├── test.yml          # Reusable test workflow
└── deploy.yml        # Reusable deployment workflow
```

## Core Principles
- DRY (Don't Repeat Yourself)
- Secure by default
- Efficient resource usage
- Clear failure points
- Reproducible builds

## Workflow Organization
- Separate CI/CD concerns
- Reusable workflow components
- Environment-specific configurations
- Clear naming conventions
- Optimized triggers

## Quality Standards
- Workflow linting
- Action versioning
- Timeout constraints
- Concurrency limits
- Required status checks

## Project Conventions
```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
    
# Define environment variables
env:
  NODE_VERSION: '18.x'
  CACHE_KEY: npm-${{ hashFiles('**/package-lock.json') }}

jobs:
  build:
    name: Build and Test
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
        
      - name: Upload artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/
```

## Security Standards
- OIDC authentication
- Secrets management
- RBAC permissions
- Security scanning
- Dependencies audit

## Testing Strategy
- Self-hosted runner tests
- Matrix testing
- Integration tests
- Environment validation
- Smoke tests

## Reusable Workflows
```yaml
# .github/workflows/reusable/deploy.yml
name: Reusable Deploy

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
    secrets:
      DEPLOY_TOKEN:
        required: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: ${{ inputs.environment }}
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: build
          
      - name: Deploy to environment
        run: ./deploy.sh
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

## Error Handling
- Clear error messages
- Retry mechanisms
- Fallback strategies
- Notification systems
- Debug artifacts

## Performance Guidelines
- Job parallelization
- Efficient caching
- Build matrices
- Action composition
- Conditional steps

## Matrix Builds
```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [16.x, 18.x, 20.x]
        exclude:
          - os: windows-latest
            node-version: 16.x
            
    steps:
      - uses: actions/checkout@v4
      
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          
      - run: npm test
```

## Environment Configuration
- Environment secrets
- Protection rules
- Deployment gates
- Approval workflows
- Variable inheritance
