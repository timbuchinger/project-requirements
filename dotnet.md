# .NET Development Guidelines

## Environment
- .NET 8 or later
- Use SDK-style project files
- Target frameworks in global.json

## Code Quality
- .editorconfig for consistency
- StyleCop.Analyzers
- Enable nullable reference types

## Testing
- xUnit for testing
- FluentAssertions for assertions
- Mock using NSubstitute
- 80% code coverage minimum

## Project Structure
- Clean Architecture pattern
- Solution folders by feature
- Separate test projects
- Common abstractions in Core

## Best Practices
- CQRS for complex domains
- Strongly typed configurations
- Dependency injection
- Async by default
