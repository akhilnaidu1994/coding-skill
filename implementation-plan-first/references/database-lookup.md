# Database Lookup

Use this file only during planning, before writing the Markdown implementation plan. Database lookup is for understanding schema and representative data; it is not an integration test, smoke test, or implementation verification step.

## Permission

Before connecting or querying, ask once per task with `ask_question` when available:

1. `Use read-only database lookup (Recommended)` - inspect schema and tiny safe samples to reduce assumptions in the implementation plan.
2. `Skip database lookup` - proceed from code/config/docs and record assumptions in the plan.
3. `Ask me for specific queries first` - request approval before each query or query group.

If `ask_question` is unavailable, ask the same question in chat with the recommended option first.

## Allowed

- Schema metadata: schemas, tables, columns, types, nullable flags.
- Relationships and constraints: primary keys, foreign keys, unique constraints, checks.
- Indexes and query-shape clues relevant to implementation.
- Safe counts, such as row counts or grouped counts, when useful for design decisions.
- Tiny representative samples with explicit limits, only when values are non-sensitive and necessary to understand shape.

## Banned

- Writes: `INSERT`, `UPDATE`, `DELETE`, `MERGE`, `TRUNCATE`.
- DDL or migration actions: `CREATE`, `ALTER`, `DROP`, schema changes.
- Stored procedure/function execution unless it is a documented read-only metadata function.
- Broad exports, unbounded queries, large result sets, or full-table scans without a tight reason and limit.
- Secrets, credentials, tokens, private customer data, or PII in the generated plan.
- Real database calls inside unit tests or as proof that implementation works.

## Plan Output

Write only concise findings that affect implementation in `Current Data Or Schema Notes`. Prefer shapes over raw values:

```markdown
## Current Data Or Schema Notes
- `orders.customer_id` is nullable in the current schema, so service validation must enforce required customer IDs before persistence.
- `users.email` has a unique constraint; duplicate handling should map repository errors to the existing conflict path.
```

If lookup is skipped, denied, or unavailable:

```markdown
## Current Data Or Schema Notes
- Database lookup was not used; plan assumes the existing repository methods and DTOs reflect the current schema.
```
