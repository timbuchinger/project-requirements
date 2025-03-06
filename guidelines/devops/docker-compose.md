# Docker Compose Guidelines

## Project Structure

```text
project/
├── docker-compose.yml          # Main compose file
├── docker-compose.override.yml # Local overrides
├── .env                       # Environment variables
└── services/                 # Service definitions
    ├── web/
    ├── api/
    └── db/

services/service-name/
├── Dockerfile
├── .env.example
└── config/
```

## Core Principles

- Service isolation
- Environment parity
- Configuration management
- Resource sharing
- Development workflow

## File Organization

- Split by environment
- Clear service dependencies
- Volume management
- Network configuration
- Secret handling

## Quality Standards

- Version pinning
- Configuration validation
- Resource constraints
- Health checks
- Logging setup

## Project Conventions

```yaml
# Base docker-compose.yml
version: '3.8'

services:
  api:
    build:
      context: ./services/api
      target: development
    volumes:
      - ./services/api:/app
      - api_node_modules:/app/node_modules
    environment:
      NODE_ENV: development
      DATABASE_URL: postgres://user:pass@db:5432/dbname
    depends_on:
      db:
        condition: service_healthy

  web:
    build: ./services/web
    volumes:
      - ./services/web:/app
      - web_node_modules:/app/node_modules
    ports:
      - "3000:3000"
    environment:
      VITE_API_URL: http://api:4000

  db:
    image: postgres:15-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 5s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
  api_node_modules:
  web_node_modules:
```

## Environment Management

```yaml
# docker-compose.override.yml
services:
  api:
    command: npm run dev
    ports:
      - "4000:4000"
    environment:
      DEBUG: "true"
      
  web:
    command: npm run dev
    environment:
      VITE_DEV_MODE: "true"

  db:
    ports:
      - "5432:5432"
```

## Security Standards

- Secret management
- Network isolation
- Service RBAC
- Volume permissions
- Health checks

## Development Workflow

```bash
# Example development workflow
docker compose up -d    # Start services
docker compose logs -f  # Watch logs
docker compose exec api npm test  # Run tests
docker compose down -v  # Cleanup
```

## Service Dependencies

```yaml
services:
  api:
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
      
  worker:
    depends_on:
      rabbitmq:
        condition: service_healthy
```

## Performance Guidelines

- Resource limits
- Volume mounts
- Network modes
- Build caching
- Development optimizations

## Error Handling

```yaml
services:
  api:
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:4000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

## Network Configuration

```yaml
services:
  api:
    networks:
      - backend
      - api_gateway
  
  web:
    networks:
      - frontend
      - api_gateway
      
  db:
    networks:
      - backend

networks:
  frontend:
  backend:
    internal: true
  api_gateway:
```

## Volume Management

```yaml
services:
  api:
    volumes:
      - type: bind
        source: ./services/api
        target: /app
      - type: volume
        source: api_node_modules
        target: /app/node_modules

volumes:
  api_node_modules:
    name: ${COMPOSE_PROJECT_NAME:-project}_api_node_modules
```

## Multi-Environment Setup

```yaml
# docker-compose.prod.yml
services:
  api:
    build:
      target: production
    restart: always
    deploy:
      replicas: 3
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
    
  nginx:
    image: nginx:alpine
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    ports:
      - "80:80"
    depends_on:
      - api
      - web
```

## Profile Usage

```yaml
services:
  api:
    profiles: ["api", "backend"]
  
  worker:
    profiles: ["worker", "backend"]
  
  storybook:
    profiles: ["frontend-dev"]

# Usage: docker compose --profile backend up
