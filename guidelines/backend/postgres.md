# PostgreSQL Guidelines

## Schema Management
- Define schemas in SQL files, not application code
- Use migrations for schema changes
- Version control all schema files
- Document table relationships
- Keep schema files in `migrations/` directory

## Best Practices
- Use appropriate data types
- Add constraints at database level
- Index frequently queried columns
- Set explicit column lengths
- Use ENUM types for fixed sets

## Performance
- Write optimized queries
- Use EXPLAIN ANALYZE
- Proper indexing strategy
- Regular VACUUM
- Keep tables normalized

## Security
- Principle of least privilege
- Use connection pooling
- Secure connection strings
- Regular user audit
- Role-based access

## Backups
- Regular full backups
- Point-in-time recovery
- Test restore procedures
- Document backup strategy
- Retain backup history

## Migrations
- One change per migration
- Reversible migrations
- Timestamp prefixed files
- Test migrations locally
- Clear migration descriptions
