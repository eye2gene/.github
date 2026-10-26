---
name: review
description: Review an Eye2Gene pull request and write comments with the org labels (blocking, suggestion, question, nit, praise), or work through review comments on your own PR. Use when asked to review a PR or respond to review feedback in an Eye2Gene repository.
disable-model-invocation: true
---

# Code review

**Reviewing**
1. Traceability first: title format, a linked issue, and an impact assessment
   that matches the diff (dependency change ↔ SOUP, data handling ↔ data
   protection). Anything missing is `blocking:`.
2. Then correctness, tests, and patient data exposure. Real data in fixtures,
   logs or error messages is `blocking:`.
3. One comment per point: `<label>: <what>, because <why>`, plus a suggested fix when it's cheap.

| Label | Meaning |
|---|---|
| `blocking:` | Must be fixed before merge |
| `suggestion:` | Optional; the author decides |
| `question:` | Needs an answer, maybe not a change |
| `nit:` | Trivial; never blocks |
| `praise:` | Worth keeping |

Show the comments; post them only when asked. Never approve: code-owner
approval is a human QMS control (SOP-25 §5.1.3.5).

**Responding to review**
For each thread, fix it and reply with the commit SHA, or reply with a reason.
Don't resolve someone else's `blocking:` thread.
