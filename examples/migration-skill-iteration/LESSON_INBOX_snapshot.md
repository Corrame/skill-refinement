# Lesson Inbox — Snapshot at 2024-11-14

This is a snapshot of the inbox at the time of the review described in this
example. It is not a live inbox.

## Entries

### 2024-11-12 - Migration applied while batch writer was active

- Source: db-migration worker, task `staging-schema-add-user-flags`, code
  writer agent
- Lesson: Before running a migration, check for active long-running writers on
  the target table. A migration that succeeds while a batch job is writing can
  leave the rollback path untested and the table in an ambiguous state.
- Evidence: `users` table had a batch import job running during the migration
  window. Migration script exited 0. Post-migration drift check found 3 rows
  with unexpected null values in the new column. Rollback script had not been
  tested and failed on the first dry run due to a missing index assumption.
- Suggested destination: db-migration skill
- Status: kept

### 2024-11-12 - Rollback not verified before apply

- Source: same task as above, post-incident review
- Lesson: Rollback script must be verified (dry-run or diff check) before the
  forward migration is applied, not after drift is detected.
- Evidence: Team spent 40 minutes reconstructing a rollback path after the fact.
  The forward script had no paired rollback check step. This is a repeatable
  cost for any future migration that deviates from happy path.
- Suggested destination: db-migration skill
- Status: kept

## Review Outcome — 2024-11-14

Both lessons reviewed by operator and main agent.

- "Check for active writers" → **promoted** into db-migration skill v2, step 2.
- "Verify rollback before apply" → **promoted** into db-migration skill v2,
  step 3.

Inbox entries archived after promotion.
