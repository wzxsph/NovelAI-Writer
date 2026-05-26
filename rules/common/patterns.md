# Common Patterns

## Repository Pattern

Encapsulate data access behind a consistent interface:
- Define standard operations: findAll, findById, create, update, delete
- Concrete implementations handle storage details
- Business logic depends on abstract interface

## API Response Format

Use a consistent envelope for all API responses:
- success/status indicator
- data payload
- error message field
- metadata for paginated responses
