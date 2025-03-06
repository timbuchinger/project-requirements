# FastAPI Architecture Guidelines

## Project Structure

```text
project_name/
├── api/                  # API endpoints
│   ├── v1/              # API version
│   └── dependencies/    # Dependency injection
├── core/                # Core functionality
│   ├── config.py       # Settings
│   ├── security.py     # Authentication
│   └── events.py       # Event handlers
├── models/              # Data models
│   ├── domain/         # Domain models
│   └── schemas/        # Pydantic schemas
├── services/           # Business logic
├── repositories/       # Data access
└── tests/              # Test suite

api/v1/
├── endpoints/          # Route handlers
├── api.py             # Router registration
└── dependencies.py    # Route dependencies
```

## Core Principles

- Async-first development
- Type safety with Pydantic
- Dependency injection
- SOLID principles
- OpenAPI documentation

## Code Organization

- Router-based API structure
- Service layer pattern
- Repository pattern for data access
- Pydantic models for validation
- Typed dependencies

## Quality Standards

- 100% type coverage
- Ruff for linting
- Black for formatting
- 80% test coverage minimum
- API documentation up-to-date

## Project Conventions

```python
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.ext.asyncio import AsyncSession
from typing import Annotated

router = APIRouter(prefix="/orders")

async def get_order_service(
    db: AsyncSession = Depends(get_db)
) -> OrderService:
    return OrderService(db)

@router.post("/")
async def create_order(
    data: CreateOrderSchema,
    service: Annotated[OrderService, Depends(get_order_service)]
):
    try:
        order = await service.create(data)
        return order
    except ValidationError as e:
        raise HTTPException(
            status_code=400,
            detail=str(e)
        )
```

## Security Standards

- OAuth2 with JWT
- Password hashing (Argon2)
- Rate limiting
- CORS configuration
- Security headers (Middleware)

## Testing Strategy

- pytest-asyncio for async tests
- Async test client
- Mock external services
- Dependency overrides
- Separate test database

## Performance Guidelines

- Async database operations
- Connection pooling
- Response caching
- Background tasks
- N+1 query prevention

## Error Handling

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse

app = FastAPI()

@app.exception_handler(DomainError)
async def domain_error_handler(
    request: Request,
    exc: DomainError
) -> JSONResponse:
    return JSONResponse(
        status_code=400,
        content={
            "message": str(exc),
            "type": exc.__class__.__name__
        }
    )

@app.middleware("http")
async def error_handling_middleware(
    request: Request,
    call_next
):
    try:
        return await call_next(request)
    except Exception as e:
        return JSONResponse(
            status_code=500,
            content={
                "message": "Internal server error",
                "type": "ServerError"
            }
        )
```

## Dependency Injection

- Use FastAPI's DI system
- Scope dependencies appropriately
- Cache heavy computations
- Layer dependencies properly
- Test with overrides
