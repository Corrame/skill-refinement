# Example: Agent Merge Case Study

This example shows the full lifecycle of merging redundant subagents. It is not
fictional — it is a sanitized version of a real project's subagent configuration
refinement.

## Starting State

The project had nine subagents defined in a single configuration file. Two pairs
showed clear overlap:

- `explore` (code exploration, deepseek-v4-flash, no custom prompt) and
  `code-reader` (read-only code detective, deepseek-v4-flash, custom prompt).
- `screenshot-reader` (visual, gemini, read-only) and `general` (miscellaneous
  worker, gemini, read-only).

## What Changed

### Merge 1: explore → code-reader

`explore` was a platform-default agent with no custom prompt. `code-reader` had
a detailed role brief with search patterns, output format, and tool restrictions.
Both were read-only and used the same model.

Decision: merge into `code-reader`, keeping the name that matched `code-writer`
for pairing, and inheriting the custom prompt and tool restrictions from the
original `code-reader`.

### Merge 2: screenshot-reader → general

`general` was a miscellaneous read-only worker with bash access. `screenshot-reader`
was a visual-only agent with the same model and read-only restriction. Both
lacked write access.

Decision: merge into `general`. The general worker gained visual capability by
inheriting the same visual-capable model. Its tool restrictions were tightened
to `write: false, edit: false`. The screenshot-reader was removed.

## Result

Nine subagents reduced to seven. Two merged pairs eliminated. The remaining
agents all have clear, non-overlapping roles:

- `general` — visual-capable read-only worker (read, view, bash)
- `code-reader` — read-only code detective
- `code-writer` — implementation worker
- Three-tier review chain (external-expert, ultra-review, final-boss-review)
- `web-researcher` — external research

Permission boundary: only `code-writer` and the main agent can write files.

## Lessons

1. **Merge criteria**: same model + same work class + no capability loss = merge.
   Do not merge if visual, write, or tool restrictions would be lost.
2. **Permission isolation**: general workers should be read-only. Write access
   creates ambiguity about who is responsible for changes.
3. **Clean boundaries**: a smaller subagent roster with clear roles is easier for
   the main agent to dispatch to than a large roster with overlapping agents.
4. **Platform quirks**: some models fail tool calls on specific platforms. Role
   briefs should include explicit required-parameter instructions for models
   affected by platform-specific tool schemas (see ROLE_BRIEF_PATTERNS.md,
   Pattern 3).
