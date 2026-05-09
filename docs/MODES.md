# Modes

The review loop can run at different levels of automation. The right mode
depends on how much trust you extend to the main agent and whether your skills
are version-controlled.

## Mode 1: Manual Inbox

Skills are plain files. The main agent and any subagents cannot edit them
directly. Lesson candidates go into `LESSON_INBOX.md` and wait for a human to
decide.

**When to use**: skills are not in version control; the team wants tight control
over what becomes doctrine; the rules are high-stakes or compliance-relevant.

**Flow**:

```text
worker -> LESSON_INBOX.md -> human reviews -> human edits SKILL.md
```

**Safety net**: human judgment before any skill changes.

---

## Mode 2: Git-Native

Skills live in a git repository. The main agent proposes skill changes as
commits or pull requests. The human reviews the diff asynchronously and merges
or closes.

**When to use**: skills are version-controlled; you trust the main agent to
draft reasonable changes; you want the promotion history recorded in git rather
than a separate inbox file.

**Flow**:

```text
worker reports lesson -> main agent drafts SKILL.md change
-> PR or commit -> human reviews diff -> merge = promote, close = reject
```

**Safety net**: git history and diff review. Any bad promotion can be reverted.
`LESSON_INBOX.md` is optional or unused in this mode.

---

## Choosing

| Condition | Mode |
|---|---|
| Skills not in version control | Manual Inbox |
| High-stakes or compliance rules | Manual Inbox |
| Skills in git, main agent trusted | Git-Native |
| Want full audit trail in git | Git-Native |
| Experimenting, not sure yet | Start with Manual Inbox |

The two modes share the same underlying loop. The difference is only in where
the review gate lives: a paper queue or a pull request.
