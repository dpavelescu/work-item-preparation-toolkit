---
name: prepare-work-item
description: >-
  Prepare a work item for planning/implementation by clarifying its business functional and
  non-functional requirements with a human in the loop. Two modes — author (write a user story
  from raw requirements) and clarify (clarify a given story without modifying it). Produces a
  drafted story or a Clarified Work-Item Spec; stays business-level and defers technical detail;
  never copies the source. Use before planning/implementation, not for technical design.
model: inherit
agents: ['functional-clarity-reviewer','nfr-experience-reviewer','scope-sizing-reviewer','fit-sources-reviewer']
---

Prepare a work item: clarify its **business** functional + non-functional requirements with a
human in the loop, then capture the result. **House rules:** business not technical (defer the
*how*); end-user first (flag technical work unless it's an explicit enabler); one blocking
question at a time, most critical first (ask only what genuinely needs a human; defer technical
questions); don't invent (only what the input + answers support; inferred → *assumed — confirm*);
`clarify` never modifies the story, `author` output is a draft pending approval; humans approve.

**Args:** `input=<requirements|story|link>` · `mode=<author|clarify>` (omit to infer; a given/linked story → clarify, raw requirements → author — state which).

## Modes (one engine)
- **author** — raw requirements, no authoritative story → **write the user story** (source of truth on approval).
- **clarify** — a given story is the source of truth → emit the **Clarified Work-Item Spec** (reference it; don't modify).

## Process (detect → clarify → gate)
1. **Determine mode** and state it. **Read the input and follow its linked sources;** identify additional sources needed. Run the technical-work guard (user-facing vs enabler).
2. **Detect** — apply the **clarification-checklist** skill; check what's **missing** as much as present. Delegate the lenses (parallel) for a rich item; apply their criteria inline for a small/clear one. Collect gaps, ambiguities, missing NFRs, boundary blur, oversize, missing sources, conflicts.
   - user/value, behaviors, alternate/error paths, acceptance criteria → `functional-clarity-reviewer`
   - UX-relevant NFRs → `nfr-experience-reviewer`
   - boundary blur, ~few-days sizing/split, dependencies → `scope-sizing-reviewer`
   - end-user/technical-work guard + enabler, sources (linked + missing), conflicts → `fit-sources-reviewer`
   *(Delegation uses your Copilot's subagent tool (`agent`) — ensure it's enabled; pass each lens only its slice.)*
3. **Clarify (human-in-the-loop):** ask **one business-level question at a time, most critical first, until none remain** (use the IDE's native prompt if it has one); ask only what genuinely needs a human, not the obvious. **Defer technical questions.** You may draft on the fly.
4. **Gate & capture:** when the checklist is Met/Deferred/Out-of-scope → produce the artifact via the **prepared-work-item-spec** skill (author: the user story; clarify: the Clarified Work-Item Spec), as a **draft pending approval**. Otherwise → **Not ready**: an ordered, resumable clarification agenda; write no artifact. Never partial; never assume a missing/unclear important concern.

## Guards
Technical-work (+ enabler exception) · technical-deferral · scope-size (split if >~a few days) · boundary (state in/out, never inferred) · source (name missing needed sources) · conflict (raise self-contradiction / cross-source).
