# Django Architecture Guidelines

## Project Structure

```text
project_name/
├── apps/                  # Django applications
│   ├── core/             # Core functionality
│   ├── users/            # User management
│   └── features/         # Feature-specific apps
├── config/               # Project configuration
│   ├── settings/        # Environment settings
│   ├── urls.py          # URL routing
│   └── wsgi.py          # WSGI configuration
├── static/              # Static files
├── templates/           # HTML templates
└── tests/               # Test suite

apps/feature_name/
├── api/                 # API views and serializers
├── migrations/          # Database migrations
├── templates/          # Feature templates
├── models.py           # Data models
├── services.py         # Business logic
├── urls.py             # URL patterns
└── views.py            # View logic
```

## Core Principles

- Fat models, thin views
- Service layer for complex business logic
- Reusable app architecture
- Class-based views
- RESTful API design

## Code Organization

- Model methods for data-related operations
- Service classes for business logic
- Serializers for data transformation
- Custom model managers for queries
- Signals for cross-cutting concerns

## Quality Standards

- 80% test coverage minimum
- Black code formatting
- Ruff for linting
- Type hints with mypy
- Django Debug Toolbar in development

## Project Conventions

```python
from django.db import models
from django.contrib.auth import get_user_model

class Order(models.Model):
    status = models.CharField(
        max_length=20,
        choices=[
            ('draft', 'Draft'),
            ('pending', 'Pending'),
            ('completed', 'Completed')
        ]
    )
    customer = models.ForeignKey(
        get_user_model(),
        on_delete=models.PROTECT
    )
    created_at = models.DateTimeField(auto_now_add=True)
    
    class Meta:
        ordering = ['-created_at']
    
    def process(self):
        # Business logic here
        pass
    
    @property
    def total(self):
        return self.items.aggregate(
            total=models.Sum('price')
        )['total'] or 0
```

## Security Standards

- Django security middleware
- CSRF protection
- Security headers configuration
- Password validation rules
- Session security settings

## Testing Strategy

- pytest as test runner
- Factory Boy for test data
- Integration tests with DB
- View testing with Django test client
- Mock external services

## Performance Guidelines

- Database query optimization
- Efficient model relationships
- Caching strategy (Redis)
- Select/Prefetch related usage
- Asynchronous tasks with Celery

## Error Handling

```python
from django.core.exceptions import ValidationError
from rest_framework.views import exception_handler
from rest_framework.response import Response

def custom_exception_handler(exc, context):
    if isinstance(exc, ValidationError):
        return Response(
            {'detail': exc.messages},
            status=400
        )

    response = exception_handler(exc, context)
    
    if response is None:
        return Response(
            {'detail': 'Internal server error'},
            status=500
        )
        
    return response
