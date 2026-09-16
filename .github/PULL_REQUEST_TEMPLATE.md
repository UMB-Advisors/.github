## What changed

<!-- One or two sentences in plain language. What does this do that the previous code did not? -->

## Why

<!-- The reason, not a restatement of the diff. Cite the spec, ADR, issue or decision it comes from. -->

## Evidence

<!-- How you know it works: the command you ran and its result, a screenshot, a deploy URL.
     If a test guards this change, say that you watched it fail before the fix. -->

## Risk and rollback

<!-- What could this break, and how would someone undo it? Name anything that is not reversible. -->

## Checklist

- [ ] Tests and lint pass locally
- [ ] Every changed line traces to the stated purpose; no unrelated refactoring
- [ ] Non-obvious behaviour has a comment naming the rule that requires it
- [ ] No credential, key or client-confidential material in the diff
