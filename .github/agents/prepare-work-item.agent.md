---
name: prepare-work-item
description: >-
  Prepare a work item for planning/implementation by clarifying its business functional and
  non-functional requirements with a human in the loop. Two modes — author (write a user story
  from raw requirements) and clarify (clarify a given story without modifying it). Produces a
  drafted story or a Clarified Work-Item Spec; stays business-level and defers technical detail;
  never copies the source. Use before planning/implementation, not for technical design.
model: inherit
agents: ['functional-clarity-reviewer','nfr-experience-reviewer','scope-sizing-reviewer','fit-sources-reviewer','consistency-reviewer']
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
1. **Determine mode** and state it. **Read the input and actually retrieve each linked source**, passing their content to the lenses; identify additional sources needed. **A critical source that can't be retrieved is blocking, like a missing one.** Run the technical-work guard (user-facing vs enabler). **Seed check (author mode):** input must carry an identifiable intended capability/outcome to clarify; if not → **Not ready — needs upstream ideation** (don't clarify a product into existence).
2. **Detect** — apply the **clarification-checklist** skill; check what's **missing** as much as present. Delegate the lenses (parallel) for a rich item; apply their criteria inline for a small/clear one. Collect gaps, ambiguities, missing NFRs, boundary blur, oversize, missing sources, conflicts.
   - user/value, behaviors, alternate/error paths, acceptance criteria → `functional-clarity-reviewer`
   - UX-relevant NFRs → `nfr-experience-reviewer`
   - boundary blur, ~few-days sizing/split, dependencies → `scope-sizing-reviewer`
   - end-user/technical-work guard + enabler, sources (linked + missing), conflicts → `fit-sources-reviewer`
   *(Delegation uses your Copilot's subagent tool (`agent`) — ensure it's enabled; pass each lens only its slice.)*
   - **Then pool the lenses' findings into one criticality-ranked agenda, collapsing questions that target the same decision into one** — the clarify loop works from this single agenda.
3. **Clarify (human-in-the-loop):** ask **one business-level question at a time, most critical first**, from the agenda; ask only what genuinely needs a human, not the obvious. After each answer, **fold it back into the agenda — resolve the item, supersede what it changes, add any new fork it opens — before picking the next**, **until the agenda is empty** (re-delegate a lens only if an answer materially reshapes a whole dimension; rare). **Defer technical questions.** You may draft on the fly. **Keep a running answer ledger and apply the consistency guard per answer** (check each answer vs prior answers **and the sources**) — surface contradictions, record supersessions explicitly, escalate circularity.
4. **Gate & capture:** before producing the artifact, run the **`consistency-reviewer`** over the assembled draft + ledger and resolve any contradiction it finds. When the checklist is Met/Deferred/Out-of-scope **and the `consistency-reviewer` passes (coherent & actionable)** → produce the artifact via the **prepared-work-item-spec** skill (author: the user story; clarify: the Clarified Work-Item Spec), as a **draft pending approval**. Otherwise → **Not ready**: an ordered, resumable clarification agenda; write no artifact. Never partial; never assume a missing/unclear important concern.

## Guards
Technical-work (+ enabler exception) · technical-deferral · scope-size (>~a few days → no artifact; propose split; each slice re-runs) · boundary (state in/out, never inferred) · source (missing/unlinked/unretrievable critical source → blocking) · conflict (within/between sources, input↔source, answer↔source — never resolve silently) · consistency (ledger per answer vs prior answers + sources; whole-spec coherence + actionability `consistency-reviewer` pass at the gate; a readiness condition) · seed (author: no capability/intent → Not ready, route upstream) · calibration (capture every critical concern, nothing more — no over/under-specification) · deferral (human may defer; minor → TBD/assumed; critical only by explicit decision; ⚠ value-impact note if it lowers value; else Not ready).
