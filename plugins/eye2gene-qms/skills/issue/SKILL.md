---
name: issue
description: Draft an Eye2Gene GitHub issue (bug or feature) using the org form fields, or route quality events to the QMS repository. Use when the user asks to file, raise or write up an issue, bug or feature request in an Eye2Gene repository.
disable-model-invocation: true
---

# Issue

**Route first.** An NCR, CAPA, complaint, security incident or data breach is
not a product issue. Say it belongs in the QMS repository (SOP-3, SOP-33) and
stop. For a security incident or breach, the Information Security Officer
(and the DPO, for personal data) must be told immediately.

**Bug** (fields of the org bug form):
- What happened: Did / Saw / Expected
- Steps to reproduce
- Version or build first observed (SOP-25 §5.6.2.1)
- Type and criticality (SOP-25 §5.6.2.2): leave for triage if unsure. Any hint
  of patient harm → Type: Safety; flag it to the human.
- Evidence: redacted or synthetic only
- Patient safety note: a candidate hazard ID or "NO MATCHING HAZARD", only if the user supplies it

**Feature:** "As a <role>, I want <action>, so that <benefit>"; why it's
needed; testable acceptance criteria.

Title: a plain description, no type prefix (labels carry the type). Show the
draft; run `gh issue create` only when asked. No patient data.
