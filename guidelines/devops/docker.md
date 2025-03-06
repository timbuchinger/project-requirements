# Docker Guidelines

## Project Structure
```
project/
├── Dockerfile          # Main Dockerfile
├── .dockerignore      # Files to exclude
├── docker/            # Docker-related files
│   ├── dev/          # Development configuration
│   └── prod/         # Production configuration
└── scripts/          # Build/helper scripts

docker/
├── dev/
│   ├── Dockerfile    # Development Dockerfile
│   └── docker-compose.yml
└── prod/
    ├── Dockerfile    # Production Dockerfile
    └── docker-compose.yml
```

## Core Principles
- Multi-stage builds
- Layer optimization
- Security by default
- Minimal base images
- Clear build stages

## Image Building
- Use specific tags
- Layer caching
- Build arguments
- Health checks
- Resource limits

## Quality Standards
- Linting (hadolint)
- Security scanning
- Best practices check
- Size optimization
- Version pinning

## Project Conventions

### Multi-stage Build Example
```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm ci --only=production
USER node
EXPOSE 3000
CMD ["npm", "start"]
```

### Development Dockerfile
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
ENV NODE_ENV=development
CMD ["npm", "run", "dev"]
```

## Security Standards
- Non-root users
- Minimal base images
- Package verification
- Secret management
- CVE scanning

## Best Practices
```dockerfile
# Example of best practices
FROM alpine:3.18

# Add metadata
LABEL maintainer="team@example.com" \
      version="1.0.0" \
      description="Example application"

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Install dependencies in one layer
RUN apk add --no-cache \
    python3 \
    py3-pip \
    && pip3 install --no-cache-dir \
    requests==2.31.0 \
    pyyaml==6.0.1

# Use specific versions
COPY --chown=appuser:appgroup . .

# Set user
USER appuser

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
    CMD wget -q --spider http://localhost:3000/health || exit 1

# Default command
CMD ["python3", "app.py"]
```

## Layer Optimization
- Combine RUN commands
- Clean up in same layer
- Order dependencies
- Use .dockerignore
- Leverage build cache

## Image Scanning
```bash
# Example scanning workflow
docker scan myapp:latest
trivy image myapp:latest
docker scout quickview myapp:latest
```

## Performance Guidelines
- Multi-stage builds
- Layer caching
- Minimal dependencies
- Appropriate base images
- Build context optimization

## Development Workflow
```yaml
# docker-compose.yml example
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: docker/dev/Dockerfile
    volumes:
      - .:/app
      - node_modules:/app/node_modules
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: development
    depends_on:
      - db
  
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: dev
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
      POSTGRES_DB: myapp
    volumes:
      - postgres_data:/var/lib/postgresql/data
    secrets:
      - db_password

volumes:
  node_modules:
  postgres_data:

secrets:
  db_password:
    file: ./secrets/db_password.txt
```

## Error Handling
- Graceful shutdowns
- Health checks
- Logging strategy
- Container restart policy
- Volume management

## Resource Management
```dockerfile
# Example of resource constraints
FROM node:18-alpine
WORKDIR /app
COPY . .

# Set memory limits
ENV NODE_OPTIONS="--max-old-space-size=512"

# Resource management at runtime
CMD ["node", "--max-old-space-size=512", "server.js"]

# Docker run example:
# docker run -m "1g" --cpus="1.5" myapp:latest
```

## Build Optimization
- Use BuildKit
- Parallel builds
- Cache mounts
- Build arguments
- Squash layers
