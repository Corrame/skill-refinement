# Why Rejected

## Decision

`rejected`

## Reason

The candidate overfits one risky pull request into a permanent prohibition.
It correctly notices that legacy modules need caution, but the proposed rule is
too broad:

- it blocks safe, necessary maintenance work;
- it treats all legacy modules as equally risky;
- it gives agents no operational check to perform;
- it turns one review concern into doctrine.

## Better Promotion

A narrower rule could be promoted instead:

> When reviewing changes to weakly tested legacy modules, require explicit
> evidence of the test strategy, rollback path, or manual verification plan
> before approval.

That version preserves the lesson without freezing all legacy work.
