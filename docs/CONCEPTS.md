# Concepts

## Skill

A skill is a reusable work method. It should explain when to use the method,
what steps to follow, what evidence to collect, and how to verify the result.

A skill is not a private memory for one agent instance. The main agent,
subagents, and future agents can all read the same skill when doing that kind
of work.

## Role Brief

A role brief defines a temporary worker's authority:

- what the worker owns;
- what it may read or edit;
- what actions are forbidden;
- what output format the main agent expects.

Role briefs answer "who are you for this task?" Skills answer "how should this
kind of work be done?"

## Lesson Candidate

A lesson candidate is a small operational observation from real work. It should
name a pitfall, failure mode, useful command, or repeated pattern.

It is not doctrine yet.

## Lesson Inbox

The lesson inbox is a low-commitment queue. It prevents two bad outcomes:

- losing useful experience in chat history;
- immediately turning every local observation into a permanent rule.

## Reviewed Promotion

Reviewed promotion is the decision to move a lesson from the inbox into a
durable artifact: a skill, role brief, project playbook, test harness note, or
handoff document.

Promotion should be done by a human operator and a capable reviewer, not by the
worker that produced the lesson.

A promoted rule is not permanent. If it starts creating avoidable cost, conflicts
with a tooling upgrade, or applies to a narrower case than expected, it can
re-enter the inbox as a lesson against itself. The same review gate decides
whether to narrow, replace, or remove it.

## Verification Depth

Verification depth is how much evidence a promoted lesson deserves after
promotion.

Not every rule needs a benchmark, regression suite, or repeated simulation.
Low-frequency skills may only need lightweight next-use observation. High-risk
or frequently reused skills may justify automated evals, regression cases, or
production metrics.

Verification depth should match reuse frequency, risk, and verification cost.
