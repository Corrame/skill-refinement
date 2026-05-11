# Role Brief Patterns

Patterns that emerged from designing and operating subagent role briefs on real
projects. These are not abstract advice — each pattern was applied, tested, and
refined through actual subagent failures and task outcomes.

---

## Pattern 1: Permission Isolation

### Problem

A general-purpose worker with write access creates ambiguity. When a task could
be handled by either the general worker or a specialist, the main agent must
decide not just *who* should do it, but *whether the worker will overstep*. A
general worker with write permission may decide to edit files when it should
have reported back for a specialist.

### Solution

Give general-purpose workers read-only access. Reserve write permission for
specialist implementation workers. The boundary is not about task scale ("a few
lines") — it is about authority. A one-line bug introduced by an unauthorized
edit is as dangerous as a hundred-line bug.

### How it looks in practice

The general worker runs `write: false, edit: false`. It can read files, view
images, and run bash commands. When it discovers a change is needed, it reports
back. The main agent decides: do it themselves, or dispatch to the code-writer.

### Why this works

- The main agent never has to wonder "did the worker already edit something?"
- Reviewers see a single write-capable surface (code-writer) rather than N.
- The general worker's task scope is unambiguous: observe and report.

### Anti-pattern

Letting general workers decide whether a change is "small enough" to make
themselves. Size is not a safety boundary. If a worker was not designed to
write, it should not write — regardless of how few lines it would change.

---

## Pattern 2: Agent Merge Criteria

### Problem

Projects accumulate subagent definitions over time. Two agents may have nearly
identical roles, models, and capabilities. The main agent wastes context
deciding between them, and role briefs drift apart with duplicated maintenance.

### Solution

Merge agents when all of these are true:
- They use the same model (no capability gap).
- Their role briefs describe the same class of work (both read code, both
  search, both review).
- The merge candidate does not lose a required capability (e.g., visual
  processing, write access, tool restrictions).

Keep agents separate when merging would remove a capability the system needs
or create an ambiguous role that is harder to dispatch to.

### How it looks in practice

A project had `explore` (code exploration) and `code-reader` (read-only code
detective). Both used the same model. Both were read-only. The code-reader had
a custom prompt with detailed exploration instructions; explore was bare.
Merged into a single `code-reader` agent inheriting the prompt and tool
restrictions.

Another project had `screenshot-reader` (visual) and `general` (miscellaneous
tasks). Both used a visual-capable model. Both were read-only. Merged into a
unified general worker with visual capability.

### Anti-pattern

Merging agents that use different models or have different tool schemas just
to reduce count. A merged agent that cannot handle all tasks originally covered
by the pre-merge set is worse than two agents.

---

## Pattern 3: Model-Aware Instructions

### Problem

Models from different providers handle tool schemas differently. A model may
fail to call a tool correctly because it does not know which parameters are
required, or because it was not trained on the tool platform's specific schema.
The main agent cannot observe this failure mode — it only sees the error.

### Solution

When a model shows repeated tool-calling failures for a specific platform, add
the required parameters to the role brief explicitly. Do not assume the model
will infer them from context or from the platform's runtime error messages.

### How it looks in practice

A Gemini model repeatedly failed to call the `bash` tool on a platform where
`description` was a required parameter. The model did not know this because
its training data predated the schema change. The fix was adding a short
"Platform Tool Schema" section to the role brief listing required parameters.
After the update, the model called the tool correctly without reminder.

### When to use

- A specific model repeatedly fails the same tool call.
- The failure is consistent across tasks.
- The fix can be expressed as a short instruction block.
- The model has enough instruction-following ability to apply the block.

### Anti-pattern

Adding every possible tool schema rule preventively. Role briefs should stay
lean. Add schema instructions only when a real failure shows they are needed.

---

## When to apply

These patterns reinforce each other:

- Permission isolation reduces the blast radius of any single subagent mistake.
- Agent merge criteria keep the subagent roster lean and maintainable.
- Model-aware instructions compensate for platform gaps without changing models.

Together they make subagent systems more predictable and easier to debug.

## Related

- `CONCEPTS.md` — vocabulary for the skill-refinement loop
- `WORKFLOW.md` — the review and promotion workflow
- `PROMOTION_CRITERIA.md` — when to promote a lesson into a skill or role brief
