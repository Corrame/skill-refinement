---
name: db-migration
description: >
  Reusable method for applying schema migrations in a staging environment.
  Use when the task is a bounded schema change with a known script.
---

# DB Migration Skill

Use this when the task is a schema migration with a known script and a
defined target environment.

## Method

1. Confirm the target environment and script path.
2. Run the migration script.
3. Verify the schema matches the expected state.
4. Report what changed.

## Avoid

- Running against production without explicit approval.
- Applying more than one migration per task.

## Output

```text
Environment:
Script applied:
Schema diff:
Verification:
Residual risk:
Lesson candidate:
```
