# TypeScript Guidelines

This document outlines TypeScript best practices and patterns for frontend development.

## Type System

- Use explicit typing over implicit 'any'
- Prefer interfaces for object definitions
- Use type aliases for union types and complex types
- Leverage generics for reusable type patterns
- Utilize utility types (Pick, Omit, Partial, etc.) 

## Error Handling

- Create custom error types
- Use discriminated unions for error states
- Implement typed error boundaries
- Type-safe error logging

## HTTP Clients

### Axios
- Preferred for most applications, including complex APIs and larger applications
- Benefits:
  - Built-in request/response interceptors
  - Automatic transforms for request/response data
  - Better TypeScript support out of the box
  - Consistent error handling
  - Request cancellation support
- Usage pattern:
  ```typescript
  interface ApiResponse<T> {
    data: T;
    status: number;
  }
  
  const api = axios.create({
    baseURL: process.env.API_URL
  });
  
  async function fetchData<T>(endpoint: string): Promise<ApiResponse<T>> {
    return await api.get<T>(endpoint);
  }
  ```

### Fetch
- Suitable for simpler applications or minimal API interaction
- Benefits:
  - Built into modern browsers
  - No additional dependencies
  - Smaller bundle size
  - Simple Promise-based API
- Usage pattern:
  ```typescript
  interface FetchResponse<T> {
    data: T;
    ok: boolean;
  }
  
  async function fetchData<T>(endpoint: string): Promise<FetchResponse<T>> {
    const response = await fetch(endpoint);
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    return { data, ok: response.ok };
  }
  ```

## Testing

- Use strict TypeScript configuration in tests
- Mock types for external dependencies
- Type assertion best practices
- Test utility types

## Configuration

- Enable strict mode
- Configure paths for module resolution
- Set appropriate lib and target options
- Use project references for monorepos

## Code Organization

- Group related types and interfaces
- Export type definitions from barrel files
- Maintain consistent naming conventions
- Document complex type relationships

## Performance Considerations

- Use const assertions
- Implement lazy loading for types
- Configure tsconfig for optimal build performance
