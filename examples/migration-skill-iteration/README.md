# Example: Migration Skill Iteration

This example walks through one complete iteration cycle for a database migration
skill. It is fictional but based on a realistic failure pattern.

## What Happened

A team had a `db-migration` skill used by a bounded subagent to apply schema
changes in a staging environment. The initial skill was written from common
sense and covered the happy path.

On a real task, the migration ran while an upstream batch job was writing to the
table. The migration script completed, but the rollback path had not been
verified. When a follow-up check found drift, the team could not cleanly revert.

A lesson candidate was appended to the inbox. After review, two rules were
promoted into the skill: verify rollback before applying, and record active
writers before touching a table.

## Files

- `v1-skill/SKILL.md` — the initial skill, written from common sense alone.
- `LESSON_INBOX_snapshot.md` — the inbox at the time of review.
- `v2-skill/SKILL.md` — the updated skill after promotion.

## What Changed

The promoted lessons added two concrete verification steps that the initial
skill left implicit. The skill is now more useful to a weaker model because the
evidence-backed steps are explicit, not assumed.
