---
name: prepared-work-item-spec
description: >-
  The structure and rules for the artifact prepare-work-item produces — a drafted user story
  (author mode) or a Clarified Work-Item Spec (clarify mode). Use when capturing the output.
  References the source; never copies it.
---

*Derived copy — canonical source is `Work-Item-Preparation-Playbook.md`; if they disagree, the playbook wins.*

One structure, **two roles**: in **author** mode it leads with the story narrative and **is** the
work item (a draft, pending approval); in **clarify** mode it **references** the existing story
and is the companion contract (the story stays the source of truth, untouched). It **never
copies** the source — it captures the *disambiguated understanding* and points to sources.

**Completeness differs by mode.** *author* → **complete and standalone**, every applicable section
substantive (N/A where one genuinely doesn't apply, not padded), meeting user-story rigor (it's
stored / used as-is downstream). *clarify* → **only what it resolves
or changes**; reference the story for what's already clear; **omit or mark N/A rather than pad**.
**Calibration (both):** capture every critical clarification and point of view and **nothing
more** — no over-specification (padding/verbosity/restating the source) and no under-specification
(dropping a concern). The checklist is fully assessed in both modes; artifact completeness ≠
checklist completeness.

```markdown
# <work-item title>
Mode: author | clarify   ·   Status: Captured | Captured with deferrals | Not ready
Work type: User-facing | Enabler   ·   Checklist: <n>/11   ·   Source of truth: <this (draft) | link to story>

## Story
*(author mode — the narrative)*
As a <persona>, I want <capability>, so that <outcome>.

## Reference sources
*(pointers, not copied)*
- Linked: <source (UI/UX spec, ADR, API/data contract, policy …)> — why relevant
- Needed but missing: <source> — what it resolves

## User & value

## Functional requirements
*(Given/When/Then; include alternate and error paths; tag each item with its origin)*

## Non-functional requirements (UX)
| NFR | Expectation | How we'd know | Source |
|---|---|---|---|

## Scope
- In scope: …
- Out of scope: …   *(stated explicitly, never inferred)*

## Acceptance criteria
*(testable, business-level)*

## Assumptions & dependencies
*(assumed — confirm; depends on …)*

## Success signals

## Deferred to next phase
*(technical clarifications, design, anything punted; ⚠ value-impact note on any critical deferral)*

## Sizing
*(fits ~a few days? if not → proposed split)*

## Open / blocking
*(only when Not ready — no artifact is produced)*
- Clarification agenda (most critical first):
  1. <question> — why it blocks · who decides
- Proposed split (if oversized):
  1. <slice — independently valuable> — each re-enters prepare-work-item as its own item
```

**Rules:** the output is a **draft pending human approval**. Tag each functional/NFR/AC item with
its **origin** — a requirement, a story section, an **external artifact** (UI/UX spec, ADR,
API/data contract, policy), or the **human answer** that resolved it (or *assumed — confirm*) — so
downstream knows authoritative vs clarified vs assumed. **Link, don't duplicate:** when an
authoritative artifact already details something, reference it under *Reference sources* and tag the
item's origin to it instead of copying its content. Keep it thin: state each rule once; defer
technical detail; don't invent.
