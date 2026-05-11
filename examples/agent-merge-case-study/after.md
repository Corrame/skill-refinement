# After: 7 Subagents

## code-reader (merged explore)
- Model: deepseek-v4-flash
- Tools: write: false, edit: false, bash: false
- Prompt: inherited original code-reader role brief
- Role: read-only code detective

## general (merged screenshot-reader)
- Model: gemini (visual-capable)
- Tools: write: false, edit: false
- Prompt: general-worker.md
- Role: general-purpose read-only worker with visual capability

*Two merges eliminated two agents. All remaining roles have clear, non-overlapping
boundaries. Only code-writer and the main agent can write files.*
