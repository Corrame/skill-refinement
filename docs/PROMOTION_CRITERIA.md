# Promotion Criteria

Use these questions before moving a lesson candidate into a durable skill or
role brief.

## Evidence

- Did this come from a real task, failure, review, or repeated cost?
- Is there enough detail for someone else to understand the trigger?
- Is the lesson still true in the current environment?

## Reuse

- Will this reduce cost in future similar tasks?
- Is it more than a one-off accident?
- Can it be phrased as an operational rule?

## Placement

- Does it describe how to do a kind of work? Put it in a skill.
- Does it describe authority, boundaries, or forbidden actions? Put it in a role
  brief.
- Does it describe current project state? Put it in a handoff or project doc.
- Does it describe verification strategy? Put it in a test harness guide.

## Verification

- How often is this lesson expected to be reused?
- What is the cost of being wrong?
- What is the cheapest useful verification method?
- Does this deserve next-use observation, a manual check, an automated eval, a
  regression test, or a production metric?
- Would the verification cost exceed the expected value of the rule?

Do not build a heavy eval loop for a skill with no expected reuse. Use
lightweight evidence for low-frequency skills, and reserve automated evals for
rules that are repeated, risky, or expensive when wrong.

## Quality

- Is it concise?
- Is it checkable?
- Does it avoid overfitting to one incident?
- Does it conflict with an existing rule?
- Would a weaker model apply it correctly?
- If the lesson came from a specialist worker, does it name that worker's native
  optimization target before generalizing the advice?

Specialists produce useful signal because their work is narrow and repeated.
They also produce local bias. A GitHub-maintenance worker may over-recommend
issue templates, pull request templates, or automation because repository
hygiene is its native frame. Review should preserve the signal while preventing
that frame from becoming universal doctrine.

If unsure, paste the candidate rule into a fresh low-context agent session and
ask how it would apply the rule. If the answer drifts, requires missing context,
or applies the rule too broadly, narrow the rule or keep it unpromoted.

## Decision Labels

- `promoted`: merged into a durable artifact.
- `kept`: useful, but not stable enough yet.
- `rejected`: too local, duplicated, wrong, stale, or unclear.
