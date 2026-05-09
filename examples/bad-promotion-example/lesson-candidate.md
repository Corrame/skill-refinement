# Lesson Candidate

## Source Task

A code-review subagent reviewed a single pull request that touched a fragile
legacy module.

## Observation

The pull request was risky because it changed legacy code with weak tests.
The subagent spent most of the review asking for extra caution around that
module.

## Candidate Rule

Never allow agents to approve changes to legacy modules.

## Why It Looked Plausible

The lesson came from a real task and named a real source of risk. It also
appeared to reduce future review failures by making the rule stricter.
