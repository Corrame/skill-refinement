# skill-refinement

Your agents do real work. That work exposes failures, edge cases, and repeated
costs. Most of that knowledge disappears into chat history.

**This repository gives it somewhere to go.**

---

If you use skills with AI agents, this repository is for you.

You already know the loop: do something, find it can be done better, write that
down, do it better next time. skill-refinement makes that loop explicit and
operational for AI agent workflows.

```text
agent does real work
-> lesson candidate surfaces
-> lesson waits for review
-> reviewed lesson updates a skill or role brief
-> next agent loads the improved method
```

Skills written only from common sense are shallow. Skills grown from real
failures are not.

## What This Solves

Without a review loop, skill updates fail in one of two ways:

**No update path.** The same mistakes repeat. The same edge cases surprise the
same agents. Experience evaporates.

**Direct self-updates.** Agents rewrite their own rules. Local accidents become
permanent doctrine. The skill drifts away from reality in a different direction.

The lesson inbox sits between these two failure modes. Lessons accumulate from
real work. A human and a capable reviewer decide what generalizes. Only reviewed
lessons become durable rules.

## How It Works

Three layers:

1. **Role brief** — who the agent is for this task, what it owns, what it must
   not do, and how it reports back.
2. **Skill** — how to perform a reusable kind of work: the method, the
   verification habit, the evidence standard.
3. **Lesson inbox** — a low-commitment queue for candidate lessons before they
   are promoted into durable rules.

Agents can use skills and append lesson candidates. They do not own skill
updates. That gate belongs to the human operator and the main agent.

## What Promoted Lessons Look Like

In practice, promoted lessons become a **local knowledge section** at the bottom
of a role brief or skill — a small set of rules that only someone who has run
this project in anger would know to write.

A real example from a production agent project:

> *Before running a migration, check for active long-running writers on the
> target table. A migration that exits 0 while a batch job is writing can leave
> the rollback path untested.*

That rule was not in the initial skill. It came from a real failure. It was
reviewed, promoted, and is now loaded by every agent that touches migrations in
that project.

See `examples/migration-skill-iteration/` for a complete before/after with the
inbox entries and promotion decision.

## Two Modes

**Manual inbox** — lessons go into `LESSON_INBOX.md`, a human reviews and edits
skills directly. Right for projects where skills are not yet version-controlled,
or where the rules are high-stakes.

**Git-native** — the main agent drafts skill changes as commits or pull
requests. Human reviews the diff. Merge is promote, close is reject. Git history
is the audit trail. `LESSON_INBOX.md` becomes optional.

Most projects start with the manual inbox and migrate toward git-native as trust
and tooling mature. See `docs/MODES.md` for the decision table.

## Quick Start

1. Copy `templates/LESSON_INBOX.md` into your agent workspace.
2. Copy only the role briefs and skills that match your actual workflow.
3. Ask agents to append one lesson candidate at the end of each real task.
4. Review the inbox periodically.
5. Promote only lessons that are repeatable, evidenced, and useful.

## Repository Contents

Read `docs/` first. Copy from `templates/` into your own workspace.

- `docs/CONCEPTS.md` — vocabulary
- `docs/WORKFLOW.md` — the operating loop
- `docs/MODES.md` — manual inbox vs git-native, with a decision table
- `docs/PROMOTION_CRITERIA.md` — review questions for promotion decisions
- `templates/LESSON_INBOX.md` — starter inbox
- `templates/worker_roles/` — role brief templates
- `templates/skills/` — skill templates
- `examples/migration-skill-iteration/` — complete iteration cycle, v1 to v2

## Not This

- Not an automatic skill updater. The review gate is the point.
- Not a claim that every project needs subagents.
- Not a replacement for human judgment.

## License

MIT. See `LICENSE`.
