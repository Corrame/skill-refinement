# Evidence-Based Skill Iteration

A practical review loop for keeping AI agent skills useful after their first
draft.

Most skill systems make it easy to create a skill. Fewer systems explain how a
skill should improve after real work exposes failures, edge cases, and repeated
costs.

This repository proposes a lightweight pattern:

```text
subagent or agent does real work
-> writes a lesson candidate
-> lesson enters a low-commitment inbox
-> human + stronger reviewer decide whether it generalizes
-> promoted lesson updates a skill, role brief, or playbook
-> future agents load the improved method
```

The core idea is simple: **skills should grow from reviewed evidence, not from
model guesswork alone.**

## Who This Is For

- People using AI coding agents with reusable skills, rules, or playbooks.
- Teams using subagents or smaller models for bounded execution work.
- Anyone who wants skills to improve without letting every local lesson become
  permanent doctrine.

This is not tied to a specific codebase, model provider, or agent framework.

## The Problem

A skill written only from common sense is often useful, but shallow. It can help
a weaker model follow a workflow, yet it may not help a stronger model much
because it mostly repeats what the stronger model already knows.

Real improvement starts when a skill absorbs evidence from practice:

- a command failed in a surprising way;
- a test harness drifted from the runtime;
- a subagent repeatedly misunderstood an ownership boundary;
- a migration exposed hidden coupling;
- a reviewer found that a rule was too broad, stale, or unsafe.

Without a review loop, skill updates usually fail in one of two ways:

- no update path, so the same mistakes repeat;
- direct self-updates, so temporary local lessons pollute long-term rules.

## The Pattern

Use three layers:

1. **Role brief**: who is doing the work, what authority they have, what they
   must not do, and how they report back.
2. **Skill**: how to perform a reusable kind of work, including method,
   evidence standard, and verification habit.
3. **Lesson inbox**: a low-commitment queue for candidate lessons from real
   tasks before they are promoted.

The review loop is:

```text
real task
-> lesson candidate
-> lesson inbox
-> reviewed promotion
-> skill / role brief / docs update
```

Subagents can use skills and suggest improvements. They should not own skill
updates. A human operator and a capable main agent should review whether a
candidate lesson is reusable, evidenced, non-duplicative, and placed in the
right durable artifact.

## Repository Contents

- `docs/CONCEPTS.md` explains the vocabulary.
- `docs/WORKFLOW.md` gives the operating loop.
- `docs/PROMOTION_CRITERIA.md` gives review questions for lesson promotion.
- `templates/LESSON_INBOX.md` is a starter inbox.
- `templates/worker_roles/` contains role brief templates.
- `templates/skills/` contains small, general-purpose skill templates.

## Non-Goals

- This is not an automatic skill updater.
- This is not a claim that every project needs subagents.
- This is not a replacement for human judgment.
- This is not legal, security, or compliance advice.

## Quick Start

1. Copy `templates/LESSON_INBOX.md` into your agent workspace.
2. Copy only the role briefs and skills that match your actual workflow.
3. Ask agents to append candidate lessons after real tasks.
4. Review the inbox periodically.
5. Promote only lessons that are repeatable, evidenced, and useful.

## License

MIT. See `LICENSE`.
