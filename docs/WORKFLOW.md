# Workflow

## 1. Assign Bounded Work

Give the worker a specific goal, allowed files or sources, forbidden actions,
expected output, and verification standard.

If a role brief exists, ask the worker to read it. If a relevant skill exists,
ask the worker to use it.

## 2. Execute And Report

The worker performs the task and returns:

- what it did;
- what evidence it used;
- what changed, if anything;
- what checks passed or failed;
- one lesson candidate, if the task exposed a reusable pattern.

## 3. Add Candidate Lessons

If a lesson may be reusable but is not obviously ready for promotion, append it
to `LESSON_INBOX.md`.

The worker may append candidates if the workflow allows it, but the worker does
not edit skills or role briefs on its own.

## 4. Review The Inbox

Periodically review each entry:

- Is it based on a real task or failure?
- Would it reduce cost next time?
- Is it general enough to reuse?
- Is it specific enough to act on?
- Does it duplicate an existing rule?
- Where should it live?

## 5. Promote Or Reject

Promote lessons into the right durable location:

- repeated method -> skill;
- worker authority or boundary -> role brief;
- project-specific state -> handoff or project docs;
- test workflow -> test harness notes;
- too local or wrong -> reject.

After promotion, clear or archive the inbox entry so the queue stays light.

## 6. Use The Updated Skill

Future agents load the improved skill. The same class of work should now cost
less, fail less often, or be easier to verify.
