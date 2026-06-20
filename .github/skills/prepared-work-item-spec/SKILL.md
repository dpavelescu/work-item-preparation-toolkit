---
name: prepared-work-item-spec
description: >-
  The structure and rules for the artifact prepare-work-item produces — a drafted user story
  (author mode) or a Clarified Work-Item Spec (clarify mode). Use when capturing the output.
  References the source; never copies it.
---

One structure, **two roles**: in **author** mode it leads with the story narrative and **is** the
work item (a draft, pending approval); in **clarify** mode it **references** the existing story
and is the companion contract (the story stays the source of truth, untouched). It **never
copies** the source — it captures the *disambiguated understanding* and points to sources.

```markdown
# <work-item title>
Mode: author | clarify   Status: Captured | Captured with deferrals | Not ready
Work type: User-facing | Enabler   Checklist: <n>/11   Source of truth: <this (draft) | link to story>

## Story            (author mode — the narrative)
As a <persona>, I want <capability>, so that <outcome>.

## Reference sources   (read these — pointers, not copied)
- <linked source> — why relevant     · Needed but missing: <source> — what it resolves

## User & value
## Functional requirements   (Given/When/Then; + alternate/error paths)   · origin tag
## Non-functional requirements (UX)   | NFR | Expectation | How we'd know | Source |
## Scope
- In scope: …
- Out of scope: …      (stated explicitly, never inferred)
## Acceptance criteria   (testable, business-level)
## Assumptions & dependencies   (assumed — confirm; depends on …)
## Success signals
## Deferred to next phase   (technical clarifications, design, anything punted)
## Sizing   (fits ~a few days? if not → proposed split)

## Open / blocking   (only when Not ready — no artifact is produced)
- Clarification agenda (most critical first): 1) <question> — why it blocks · who decides
```

**Rules:** the output is a **draft pending human approval**. Tag each functional/NFR/AC item with
its **origin** — a requirement, a story section, or the **human answer** that resolved it (or
*assumed — confirm*) — so downstream knows authoritative vs clarified vs assumed. Keep it thin:
state each rule once; defer technical detail; don't invent.
