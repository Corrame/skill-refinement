# skill-refinement

Subagents learn things your system forgets.

`skill-refinement` is a review loop for turning real subagent failures, repeated
costs, and edge cases into reviewed improvements to reusable skills and role
briefs.

It is not about remembering more. It is about deciding what deserves to become
instruction.

No CLI is required. The protocol runs on text files and review: ask an agent to
read this README, copy the lesson inbox into your workspace, and append one
lesson candidate after each bounded task.

---

If you use skills with AI agents, this repository is for you.

The loop only turns if subagents do bounded, repeated work. A subagent runs
the same class of task many times — code review, migration, research, test
harness. That repetition is what produces lessons. A main agent handling
one-off decisions is too varied to generate reliable signal.

```text
🤖 subagent does bounded, repeated work
        ↓
📝 lesson candidate surfaces from real failure or cost
        ↓
📬 lesson waits for review
        ↓
🚪 review gate decides what generalizes
        ↓
✅ reviewed lesson updates a skill or role brief
        ↓
🤖 next subagent loads the improved method
```

A minimal lesson candidate looks like this:

```md
Task: database migration review
Observed: migration passed locally, but no check existed for active batch writers
Candidate rule: before migration, check active long-running writers on target tables
Decision: promoted to migration skill
```

Skills written only from common sense are shallow. Skills grown from real
failures are not.

## Use With Any Agent

This repository does not require a framework, runtime, or package install. It is
an agent-native working convention.

1. Copy `templates/LESSON_INBOX.md` into your agent workspace.
2. Ask your agent to read this README.
3. Use role briefs and skills for bounded, repeated work.
4. Ask agents to append one lesson candidate after each task.
5. Review the inbox before changing any skill or role brief.
6. Promote only lessons that are evidenced, repeatable, and useful.
7. Load the improved skill or role brief next time.

Text files are the deployment surface. Review is the safety mechanism.

## Why This Works

A skill is a hypothesis. It says: "doing it this way produces good results."

Real work is the test. When an agent follows the skill and something breaks,
drifts, or costs more than expected, that is data. The skill was wrong, or
incomplete, or right in a narrower range than assumed.

The refinement loop treats that data seriously:

```
💡 Hypothesize   →   write the skill from your best current understanding
        ↑
        |
🔁 Repeat        ←   does the framework still hold?
        |                  yes → small update   no → rethink from scratch
        ↓
🔨 Test          →   let agents use it on real tasks
        ↓
📋 Collect       →   what worked, what failed, what surprised
        ↓
✏️  Update        →   promote the lesson into the skill
```

This is not a new idea. It is how any field improves when it takes evidence
seriously. The only thing new here is applying it deliberately to AI agent
skills, where the feedback loop is fast and the cost of repeated mistakes is
real.

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

Subagents use skills and append lesson candidates. They do not own skill
updates. That gate belongs to the human operator — or a main agent if your
workflow has one. The subagent's job is to do the work and report what it
learned. Promotion is someone else's decision.

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

A more mature role brief starts to accumulate local rules like these:

> Use `.venv/bin/python` for all local validation. Do not assume `python` or
> `python3` resolves to the project virtualenv.
>
> Before writing a test call, verify the target function's current signature
> and keyword-only arguments directly in source. Do not reconstruct the call
> from memory or documentation — both drift.
>
> Integration tests that rely on a hardcoded repo-root `tmp_path` will silently
> change semantics when runtime path handling becomes profile-aware. When
> touching path isolation, pass an explicit root to any affected test fixtures.

These are not general slogans. They are project-shaped rules produced by
repeated work, failure attribution, and review.

See `examples/migration-skill-iteration/` for a complete before/after with the
inbox entries and promotion decision. See `examples/mature-role-brief/` and
`examples/mature-skill/` for examples of mature artifacts after multiple
promoted lessons.

## Two Modes

**Manual inbox** — lessons go into `LESSON_INBOX.md`, a human reviews and edits
skills directly. Right for projects where skills are not yet version-controlled,
or where the rules are high-stakes.

**Git-native** — skill changes are proposed as commits or pull requests, by you
or an agent you trust with that task. Human reviews the diff. Merge is promote,
close is reject. Git history is the audit trail. `LESSON_INBOX.md` becomes
optional.

Most projects start with the manual inbox and migrate toward git-native as trust
and tooling mature. See `docs/MODES.md` for the decision table.

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
- `examples/mature-role-brief/` — what a role brief looks like after real tasks
  have promoted lessons into it
- `examples/mature-skill/` — what a skill looks like after repeated promotion
- `examples/bad-promotion-example/` — a lesson that should not be promoted

## Not This

- Not an automatic skill updater. The review gate is the point.
- Not useful without subagents doing repeated, bounded work. That repetition
  is the signal source. Without it, the loop has nothing to turn on.
- Not a replacement for human judgment.
- Not a CLI-first tool. It is a text-first protocol that any capable agent can
  read and operate.
- Not self-applying. This repository is refined through issues, pull requests,
  and maintainer review, not through the loop it describes.

## Discussion

Initial public note: https://x.com/HomuraTokido/status/2052946438288802256

Related architecture question: https://github.com/NousResearch/hermes-agent/issues/21303

## License

MIT. See `LICENSE`.
