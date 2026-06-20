---
name: prepare-work-item
description: >-
  Prepare a work item for planning/implementation by clarifying its business functional and
  non-functional requirements with a human in the loop. Two modes — author (write a user story
  from raw requirements) and clarify (clarify a given story without modifying it). Produces a
  drafted story or a Clarified Work-Item Spec; stays business-level and defers technical detail;
  never copies the source. Use before planning/implementation, not for technical design.
---

# Skill: prepare-work-item

## Invocation
```
/prepare-work-item input=<requirements|story|link> [mode=<author|clarify>]
```
If `mode` is omitted, **infer it** (a given/linked story → `clarify`; raw requirements → `author`)
and **state which mode** is in effect.

## House rules
- **Business, not technical** — clarify *what / why / for whom*; defer the *how*.
- **End-user first** — flag work that looks technical unless it's an **explicit enabler**.
- **One blocking question at a time, most critical first** — ask only what genuinely needs a human, not the obvious; **defer technical questions**.
- **Don't invent** — only what the input + answers support; inferred items marked *assumed — confirm*.
- **Source of truth** — `clarify` never modifies the story; `author` output is a **draft pending approval**.
- **No silent assumptions / no silent governance** — humans approve.

## Modes (one engine)
- **author** — raw requirements, no authoritative story → **write the user story** (becomes the source of truth on approval).
- **clarify** — a given story is the source of truth → emit the **Clarified Work-Item Spec** (reference it; don't modify it).

The detection + clarification engine below is identical; only the input and the captured artifact differ.

## Process (detect → clarify → gate)
1. **Determine mode** and state it. **Read the input and follow its linked sources;** identify additional sources needed for understanding. Run the **technical-work guard** (classify user-facing vs explicit enabler).
2. **Detect** against the **checklist** — checking what's **missing** as much as what's present. Delegate the **lenses** (parallel) for a rich item; **apply their criteria inline** for a small/clear one (right-size). Collect gaps, ambiguities (>1 reading), missing NFRs, boundary blur, oversize, missing sources, conflicts — with candidate questions.
3. **Clarify (human-in-the-loop).** Ask the **single most critical** business-level question (use the IDE's native prompt if it has one), wait, then the next — **until none remain.** Ask only what genuinely needs a human. **Defer technical questions** to the next phase. You may draft on the fly.
4. **Gate & capture.** When the checklist is **Met / Deferred / Out-of-scope** → produce the artifact (author: the **user story**; clarify: the **Clarified Work-Item Spec**) as a **draft pending approval**. Otherwise → **Not ready**: an ordered, resumable clarification agenda; **write no artifact**. Never partial; never substitute an assumption for a missing/unclear important concern.

## Clarification checklist (the measure)
*Inlined copy — canonical source is the playbook (Checklist + Guards + Spec); if they disagree, the playbook wins.*
Each item is **Met / Deferred / Open** — the consistent measure of *what to clarify* and *when it's clarified enough*:
1. User & value · 2. Functional behavior (scenarios incl. alternate/error paths) · 3. UX-relevant NFRs · 4. Scope (in / out, stated not inferred) · 5. Acceptance criteria · 6. Assumptions & dependencies · 7. Success signals · 8. Sizing (~a few days; split if oversized) · 9. Reference sources (linked + missing) · 10. No unresolved blocking ambiguity · 11. Work type (user-facing | enabler).

## Lenses (delegate when broad; inline when narrow)
| Dimension | Reviewer |
|---|---|
| user/value, behaviors, alternate/error paths, acceptance criteria | `functional-clarity-reviewer` |
| UX-relevant NFRs (perf, a11y, privacy/consent, i18n, security-as-felt, compliance, audit) | `nfr-experience-reviewer` |
| boundary blur (in/out), ~few-days sizing/split, dependencies | `scope-sizing-reviewer` |
| end-user/technical-work guard + enabler, linked + missing sources, conflicts | `fit-sources-reviewer` |
Each lens is read-only, business-level, defers technical detail, and returns findings + suggested questions. Pass each only its slice.

## Guards
Technical-work (+ enabler exception) · technical-deferral · scope-size (split if >~a few days) · boundary (state in/out) · source (name missing needed sources) · conflict (raise self-contradiction / cross-source). See the playbook.

## Output (two roles; never copies the source)
- **author** → the **user story** (it *is* the work item; a draft, pending approval).
- **clarify** → the **Clarified Work-Item Spec** (references the story; companion contract).

```markdown
# <work-item title>
Mode: author | clarify   Status: Captured | Captured with deferrals | Not ready
Work type: User-facing | Enabler   Checklist: <n>/11   Source of truth: <this (draft) | link to story>

## Story            (author mode — narrative)
As a <persona>, I want <capability>, so that <outcome>.

## Reference sources   - <linked source> — why relevant   · Needed but missing: <source> — what it resolves
## User & value
## Functional requirements   (Given/When/Then; + alternate/error paths)   · source/answer tag
## Non-functional requirements (UX)   | NFR | Expectation | How we'd know | Source |
## Scope               - In scope: …   - Out of scope: …   (stated, not inferred)
## Acceptance criteria (testable, business-level)
## Assumptions & dependencies   (assumed — confirm; depends on …)
## Success signals
## Deferred to next phase   (technical clarifications, design, anything punted)
## Sizing   (fits ~a few days? if not → proposed split)

## Open / blocking   (only if Not ready)
- Clarification agenda (most critical first): 1) <question> — why it blocks · who decides
```
Tag each functional/NFR/AC item with its origin (requirement, story §, human answer, or *assumed — confirm*).
