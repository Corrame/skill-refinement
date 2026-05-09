---
name: code-writer
description: >
  Mature reusable method for bounded implementation with verification habits
  refined from repeated code-writing tasks.
---

# Mature Code Writer Skill

Use this for focused implementation work where the spec is bounded, ownership is
clear, and the result can be verified with tests or equivalent checks.

## Method

1. Restate the spec, owned files, forbidden scope, and verification target.
2. Read the smallest source and test slices that can prove the change.
3. Inspect current signatures, fixtures, and runtime entry points before writing
   calls or patches.
4. Make the minimal change that satisfies the spec.
5. Run the closest relevant check first.
6. Attribute failures before changing more code.
7. Escalate if the fix requires product, architecture, or ownership decisions.
8. Report changed files, checks run, failures handled, residual risk, and one
   lesson candidate if the task exposed a reusable pattern.

## Avoid

- Expanding scope because adjacent code looks messy.
- Committing from a worker context.
- Treating static checks as a substitute for behavior tests when behavior tests
  exist.
- Rewriting tests to match accidental behavior before confirming intended
  behavior.
- Suppressing import, fixture, or collection failures without explaining the
  underlying dependency or wiring issue.

## Output

```text
Spec:
Files changed:
Validation:
Failures handled:
Residual risk:
Lesson candidate:
```

---

## Local Knowledge

*This section contains rules promoted from repeated implementation tasks. They
are examples of project-shaped knowledge that should not be guessed from a
fresh prompt alone.*

- Run local validation with `.venv/bin/python` unless the project explicitly
  documents another interpreter path. Shell `python` may point outside the
  project environment.

- Before adding or updating a call site, inspect the live source for the target
  function's current signature and keyword-only arguments. Do not rely on memory
  or stale documentation.

- When changing runtime paths, entry points, dependency wiring, or environment
  loading, run the full test suite after focused tests pass. These changes can
  break modules that focused tests do not import.

- If test collection fails because an optional dependency is missing, decide
  whether the dependency belongs in dev extras or whether the import should be
  lazy. Do not hide the failure by skipping collection.

- When path behavior becomes profile-aware, tests that rely on hardcoded
  repo-root `tmp_path` fixtures may silently test the wrong semantics. Pass an
  explicit root into affected fixtures.

- If a focused fix fails three times in the same area, stop widening the patch
  and report the failure boundary. Repeated local edits can turn a small bug fix
  into an accidental refactor.
