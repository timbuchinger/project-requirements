# .NET Architecture Guidelines

## Project Structure

```text
src/
├── Domain/                 # Domain entities, interfaces, exceptions
├── Application/           # Use cases, DTOs, interfaces
├── Infrastructure/        # External concerns, data access, messaging
└── API/                   # API endpoints, middleware, configuration

tests/
├── Unit/                 # Unit tests
├── Integration/          # Integration tests
└── E2E/                 # End-to-end tests
```

## Core Principles

- Clean Architecture with clear dependency flow
- Domain-Driven Design for complex business logic
- CQRS pattern for read/write operations
- Dependency Injection for loose coupling
- Vertical slice architecture for features

## Code Organization

- One aggregate root per bounded context
- Rich domain models with encapsulated logic
- Immutable value objects for domain concepts
- Domain events for cross-boundary communication

## Quality Standards

- 80% code coverage minimum
- StyleCop.Analyzers enforcement
- .editorconfig for consistent style
- XML documentation for public APIs

## Project Conventions

```csharp
public class OrderAggregate : IAggregateRoot
{
    private readonly List<OrderItem> _items;
    
    private OrderAggregate() { }  // For EF Core
    
    public static OrderAggregate Create(CustomerId customerId)
    {
        return new OrderAggregate
        {
            Id = OrderId.New(),
            CustomerId = customerId,
            Status = OrderStatus.Draft
        };
    }
    
    public void AddItem(ProductId productId, int quantity)
    {
        // Domain logic and invariant checks
    }
}
```

## Security Standards

- JWT-based authentication
- Role-based authorization
- Data encryption at rest
- Input validation using FluentValidation
- HTTPS enforcement

## Testing Strategy

- Use xUnit for testing
- AAA pattern (Arrange-Act-Assert)
- Mock external dependencies
- Integration tests with test containers
- Separate test projects per type

## Performance Guidelines

- Async/await for I/O operations
- Response compression
- Output caching where appropriate
- Efficient LINQ queries
- Monitoring with Application Insights

## Error Handling

```csharp
public class GlobalExceptionHandler : IExceptionHandler
{
    public async ValueTask<bool> TryHandleAsync(
        HttpContext context,
        Exception exception,
        CancellationToken ct)
    {
        var error = exception switch
        {
            DomainException dx => new ProblemDetails
            {
                Status = 400,
                Title = "Domain Rule Violation",
                Detail = dx.Message
            },
            NotFoundException nx => new ProblemDetails
            {
                Status = 404,
                Title = "Resource Not Found",
                Detail = nx.Message
            },
            _ => new ProblemDetails
            {
                Status = 500,
                Title = "Server Error"
            }
        };

        context.Response.StatusCode = error.Status.Value;
        await context.Response.WriteAsJsonAsync(error, ct);
        return true;
    }
}
