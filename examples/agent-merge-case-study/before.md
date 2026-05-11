# Before: 9 Subagents

## explore
- Model: deepseek-v4-flash
- Tools: default (write, edit, bash all available)
- Prompt: none
- Role: platform-default code explorer

## code-reader
- Model: deepseek-v4-flash
- Tools: write: false, edit: false, bash: false
- Prompt: custom role brief with search patterns and output format
- Role: read-only code detective

*Problem: same model, same work class. explore had no prompt; code-reader had a
detailed one. Main agent had two agents for one job.*

## general
- Model: gemini (visual-capable)
- Tools: default
- Prompt: general-worker.md (mechanical task worker)
- Role: miscellaneous read-only worker

## screenshot-reader
- Model: gemini (visual-capable)
- Tools: write: false, edit: false, bash: false
- Prompt: none
- Role: screenshot viewer

*Problem: same model, both read-only. screenshot-reader was a single-purpose
agent when general already had the same visual-capable model.*
