---
name: code-review
description: Team code-review rubric — invoke when the user asks to review a diff, PR, or branch. Applies correctness-first ordering, flags dangerous change patterns, and enforces our house style before nitpicks.
---

# code-review

Team-standard code review for pull requests, staged diffs, or a named branch.
Findings are ordered by risk — a real correctness bug always outranks a style nit.

## When to invoke

- User says "review this PR", "review the diff", "check this branch", or pastes
  a diff / PR URL and asks for feedback.
- After `/10x-implement` finishes a phase and before merge.

## Review order (fail-fast)

1. **Correctness** — will this produce the wrong result, crash, or corrupt
   data? Concrete failure scenario required for every claim.
2. **Security** — injection, secrets in code/logs, authz bypass, unsafe deserialization.
3. **Data safety** — migrations without rollback, destructive ops on shared
   state, cache/DB writes without idempotency.
4. **API/contract changes** — breaking changes to public surfaces without a
   version bump or deprecation path.
5. **Reuse & simplification** — duplicated logic that already exists, premature
   abstraction, dead code left behind.
6. **Style** — only after everything above is clean.

## What to reject outright

- `--no-verify`, `--force-with-lease` on shared branches, or disabling signing
  without an explicit reason in the PR body.
- New error swallowing (`catch {}`, bare `except:`) without a comment naming
  the specific error class being tolerated.
- Comments that only restate the code (`// increment i by 1`).
- New files under `context/` or `.claude/` created by hand instead of via the
  standard `/10x-*` skills.

## Output shape

Return findings as a numbered list, most-severe first:

```
1. [category] short claim (file:line)
   Why it's wrong: <one sentence>
   Failure scenario: <concrete inputs → wrong output>
   Suggested fix: <specific edit, not "consider refactoring">
```

If nothing survives verification, say so plainly — do not pad with speculation.
