# Work‑Item Preparation — Playbook

**Audience:** Product owners, business analysts, tech leads, developers, QA.

> **Canonical source.** This playbook is the single source of truth for the **clarification checklist**, the **guards**, and the **Prepared Work‑Item Spec** structure. The per‑tool copies (`.claude/skills/`, `.claude/agents/`, `.github/skills/`, `.github/agents/`) are derived for packaging — when any of them disagrees with this file, **this file wins** and the copy is the bug. Update here first, then propagate.

---

## Tool scope & terms

This playbook is the **tool‑neutral reference** — it describes the capability by what it *does*: one **orchestrator** (`prepare-work-item`), five **reviewer lenses** (four that detect gaps in the input + one that checks the assembled draft for consistency at the gate), and two shared **helpers** (a clarification checklist and the output spec). Packaging differs per coding agent:

- **Claude Code** — the orchestrator is a *skill* (`.claude/skills/`), the lenses are *sub-agents* (`.claude/agents/`); invoked as a `/slash` command.
- **GitHub Copilot** — all six are *custom agents* (`.github/agents/`) plus two *Agent Skills* (`.github/skills/`); invoked from the agent picker.

Examples use the Claude form. Read **"skill"** as "the workflow" and **"sub-agent"** as "the delegated lens."

---

## The one idea

> **Remove business ambiguity from a work item before planning or implementation — by authoring it from requirements, or clarifying a given story — and capture the result as a contract the next phase builds against.**

It clarifies *what / why / for whom* (functional + non‑functional, end‑user relevant). It does **not** specify *how* (that's planning/implementation), and it does **not** copy the source story.

---

## Two modes (one engine)

The clarification engine is identical; only the two ends differ.

| | **author** | **clarify** |
|---|---|---|
| Input | raw requirements (formal / draft / free text); no authoritative story | an existing story that **is** the source of truth |
| Output | a **drafted user story** (becomes the source of truth on approval) | a **Clarified Work‑Item Spec** (companion; references the story, never modifies it) |
| Source of truth | the artifact it writes (draft, pending approval) | the existing story (left untouched) |

**Mode selection:** infer it (a given/linked story → *clarify*; raw requirements → *author*) and **state which mode** is in effect; allow an explicit override. Everything between the two ends — checklist, lenses, guards, the interactive loop — is shared and mode‑agnostic.

---

## Principles

1. **Business, not technical.** Clarify the user‑facing behavior and expectations; defer the *how*.
2. **End‑user first.** Every requirement traces to a user/persona and an outcome.
3. **Human‑in‑the‑loop, one question at a time**, ordered by business criticality; ask only what genuinely needs a human and isn't obvious or already answered.
4. **Don't invent (anti‑gold‑plating).** Capture only what the input + answers support; mark inferences *assumed — confirm*.
5. **Source‑of‑truth discipline.** Reference linked sources; in *clarify* mode never modify the story.
6. **Gate, don't dump.** Produce the artifact only when the checklist is satisfied (or items are explicitly deferred/out‑of‑scope) **and the draft is internally consistent** — never a partial artifact with a list of open points.
7. **Right‑sized.** A work item should be ~a few days so downstream plans/PRs stay reviewable.

---

## The clarification checklist (the measure)

Readiness is **measured**, not guessed. Each item is **Met / Deferred / Open** — this is the consistent way to scope the clarification and to know when it's *clarified enough* to capture. It is a means, not the deliverable.

1. **User & value** — persona(s) and the outcome are explicit.
2. **Functional behavior** — observable behaviors as scenarios (happy path + meaningful alternates + user‑visible error/edge cases).
3. **Non‑functional (UX‑relevant)** — only NFRs that affect the end‑user experience, each as a *business expectation* (measurable where possible): responsiveness/perceived performance, availability‑as‑felt, accessibility, localization, privacy & data handling/consent, security‑as‑felt, compliance, auditability.
4. **Scope boundaries** — explicit **in‑scope** and **out‑of‑scope** (out‑of‑scope is *stated*, never inferred).
5. **Acceptance criteria** — testable, business‑level (Given/When/Then).
6. **Assumptions & dependencies** — business‑level, named (incl. cross‑team/other‑item deps).
7. **Success signals** — how we'll know it delivered value, where applicable.
8. **Sizing** — fits ~a few days; if not, a split is proposed.
9. **Reference sources** — the item's linked sources are considered, and any *additional* source needed for understanding is named (provided or flagged).
10. **No unresolved blocking ambiguity** — every business‑critical question is answered, deferred, or out‑of‑scope.
11. **Work type** — user‑facing (default) or explicitly an enabler.

---

## Guards

- **Technical‑work guard.** Detect signals the item is technical, not user‑facing (named tech/infra/refactor; framed as a task not an outcome; no identifiable user/value). If detected **and not explicitly an enabler** → surface it (*"This reads as technical — state the end‑user value, or mark it an enabler."*).
- **Enabler exception.** If the item is **explicitly** an enabler, switch to enabler mode: relax the user‑value requirement; instead clarify the enabler's *purpose, the capabilities/consumers it unblocks, and its acceptance*.
- **Technical‑deferral guard.** In user‑facing mode, **any technical clarification is deferred to the next phase** (recorded, not answered).
- **Scope‑size guard.** If the item is larger than ~a few days, **don't produce a final artifact** — propose candidate slices, each independently valuable.
- **Boundary guard.** When scope is blurry, clarify and **state in‑scope and the consequent out‑of‑scope explicitly**.
- **Source guard.** If a business‑critical point needs a source that's missing, unlinked, or **unretrievable**, make it a clarification item (a load‑bearing source that's missing or can't be retrieved is blocking).
- **Conflict guard.** If the input contradicts itself or a known source, raise it — don't resolve it silently.
- **Seed guard (author mode).** Author *elicits* direction that exists; it never *invents* it. If the input carries no identifiable capability/intent to clarify → **Not ready**, route upstream to ideation — don't clarify a product into existence.
- **Calibration guard (both modes).** Aim for the right level of specification: capture every load‑bearing clarification and point of view, and **nothing more**. Over‑specification (padding, verbosity, restating the obvious or the source) buries the signal; under‑specification drops a concern that downstream then has to assume. Neither is acceptable.
- **Consistency guard.** Clarifications accumulate; keep them coherent. *In the loop:* maintain a running **answer ledger** and check each new answer against it — if it **contradicts** a prior one, surface it (*"this conflicts with your earlier answer that X — which holds?"*) instead of silently absorbing it; if it **legitimately revises** a prior decision, record the **supersession explicitly** (the old answer is marked superseded, never silently dropped); if the same decision keeps flip‑flopping, **stop and escalate** (circularity). *At the gate:* run a whole‑spec consistency pass (the `consistency-reviewer` lens) over the assembled draft — no two requirements, ACs, NFRs, or scope statements may contradict. **Internal consistency is a readiness condition**, not just a writing nicety.

---

## The process (detect → clarify → gate)

1. **Determine mode** (author vs clarify) and state it. **Read the input and actually retrieve and read each linked source** (the orchestrator fetches; it passes the *content*, not just links, to the lenses); identify additional sources needed. **If a load‑bearing linked source can't be retrieved (no access, broken, paywalled), treat it like a missing one — a blocking clarification item.** Run the **technical‑work guard** (classify user‑facing vs enabler). **Seed check (author mode):** the input must carry an identifiable intended capability/outcome to clarify; if it doesn't, stop with **Not ready — needs upstream ideation** (don't clarify a product into existence).
2. **Detect** against the checklist — check what's **missing** as much as what's present. Delegate the lenses (parallel) for a rich item; **apply their criteria inline for a small/clear one** (right‑size). Findings: gaps, ambiguity (>1 reading), missing NFRs, boundary blur, oversize, missing sources, conflicts — with candidate questions. **Then pool the lenses' findings into one criticality‑ranked agenda, collapsing questions that target the same decision into one.** This single agenda is what the clarify loop works from.
3. **Clarify (human‑in‑the‑loop).** Ask **one business‑level question at a time, most critical first, until the agenda is empty** (use the IDE's native prompt if available; ask only what genuinely needs a human). **Defer technical questions.** You may draft on the fly. After each answer, **fold it back into the agenda — resolve the item, supersede what it changes, and add any new fork it opens — before picking the next** (re‑delegate a lens only if an answer materially reshapes a whole dimension; rare). **Keep a running answer ledger and apply the consistency guard per answer** (surface contradictions, record supersessions, escalate circularity).
4. **Gate & capture.** Before producing the artifact, run the **whole‑spec consistency pass** (`consistency-reviewer`) over the assembled draft; resolve any contradiction it finds (loop back if needed).
   - Checklist Met/Deferred/Out‑of‑scope **and consistency clean** → produce the artifact (author: the **user story**; clarify: the **Clarified Work‑Item Spec**) as a **draft pending approval**.
   - Otherwise → **Not ready**: an ordered, resumable clarification agenda; **no artifact**.

---

## The output: the Prepared Work‑Item Spec

A predefined structure with **two roles**: in *author* mode it leads with the **story narrative** and **is** the work item; in *clarify* mode it **references** the external story and is the companion contract. It never copies the source.

**Completeness by mode.** *author* writes a **complete, standalone** user story meeting user‑story rigor — every applicable section substantive. *clarify* writes **only what it resolves or changes**, references the story for what's already clear, and **omits or marks N/A rather than padding**. The checklist is fully assessed in both modes; artifact completeness ≠ checklist completeness. (The **calibration guard** sets the level: every load‑bearing concern, nothing more.)

**Don't duplicate external sources — link them.** When an authoritative artifact already details something (UI/UX spec, ADR, API/data contract, policy), reference it under *Reference sources* and tag the item's origin to it, instead of copying its content into the story.

```markdown
# <work-item title>
Mode: author | clarify    Status: Captured | Captured with deferrals | Not ready
Work type: User-facing | Enabler    Checklist: <n>/11    Source of truth: <this (draft) | link to story>

## Story            (author mode — the narrative)
As a <persona>, I want <capability>, so that <outcome>.

## Reference sources  (read these — pointers, not copied)
- <linked source (UI/UX spec, ADR, API/data contract, policy …)> — why relevant   · Needed but missing: <source> — what it resolves

## User & value
## Functional requirements        (Given/When/Then; + alternate/error paths)   · source/answer tag
## Non-functional requirements (UX) | NFR | Expectation | How we'd know | Source |
## Scope                          - In scope: …   - Out of scope: …  (stated, not inferred)
## Acceptance criteria            (testable, business-level)
## Assumptions & dependencies     (assumed — confirm; depends on …)
## Success signals
## Deferred to next phase          (technical clarifications, design, anything punted)
## Sizing                          (fits ~a few days? if not → proposed split)

## Open / blocking  (only if Not ready)
- Clarification agenda (most critical first): 1) <question> — why it blocks · who decides
```

**Traceability:** each functional/NFR/AC item is tagged with its origin — a requirement, a story section, an **external artifact** (UI/UX spec, ADR, API/data contract, policy), or the **human answer** that resolved it (or *assumed — confirm*) — so downstream knows what's authoritative vs clarified vs assumed.

---

## Output states
- **Captured** — checklist satisfied; downstream may proceed.
- **Captured with deferrals** — produced with explicit *Deferred* / *assumed — confirm* items that don't block.
- **Not ready** — no artifact; a resumable, criticality‑ordered clarification agenda (answer + re‑run, or answer interactively).

## Contract semantics (downstream)
- Planning/implementation **build to the artifact**: honor scope (esp. *out of scope*), satisfy the acceptance criteria, meet the NFR expectations, and **resolve the *Deferred* items in their phase**.
- If a downstream step finds it **wrong, ambiguous, or contradicted by the source** → **loop back**, don't silently reinterpret.
- The **source stays authoritative**: if the story (clarify) or requirements (author) change, re‑validate.

## Edge cases
- **Enabler/technical item:** explicit flag → enabler mode. Unflagged‑but‑technical → guard surfaces it.
- **Oversized:** never produce a final artifact; propose a split.
- **Self‑contradiction / cross‑source conflict:** raise it; don't resolve silently.
- **Sparse input:** clarify the load‑bearing gaps; mark safe inferences *assumed — confirm*; don't gold‑plate.

---

## Components & the agent / subagent / skill split

- **Orchestrator** (`prepare-work-item`) — the **interactive entry point** (a Claude *skill* / a Copilot *custom agent*), because the clarification is human‑in‑the‑loop. It determines mode, detects (delegating to lenses), runs the question loop, gates, and captures the artifact.
- **Detect lenses** (4 **sub‑agents**, read‑only, parallel, flat) — independent, non‑interactive analyses that each find gaps in one dimension of the **input** and return findings; they fire selectively (inline for small items). They map to the checklist:
  - `functional-clarity-reviewer` — user/value, behaviors, alternate/error paths, acceptance criteria.
  - `nfr-experience-reviewer` — UX‑relevant NFRs that are missing or vague.
  - `scope-sizing-reviewer` — boundary blur (in/out), ~few‑days sizing/split, dependencies.
  - `fit-sources-reviewer` — end‑user/technical‑work guard + enabler, linked + missing sources, conflicts.
- **Gate lens** (1 **sub‑agent**, read‑only) — `consistency-reviewer` fires once at the **gate**, over the **assembled draft** (not the input), checking the artifact for internal contradictions and supersession integrity before capture. (Per‑answer consistency during the loop stays in the orchestrator — it is stateful and may need to ask the human — so only the whole‑spec pass is delegated.)
- **Helpers** (2 **skills**, reused verbatim) — `clarification-checklist` (the measure) and `prepared-work-item-spec` (the output structure). *(Claude inlines these in the orchestrator/lenses; Copilot extracts them as Agent Skills.)*

The interactive clarification protocol stays **in the orchestrator** (single, interactive driver — not a skill). Each **detect** guard lives in its lens; the **cross‑cutting** guards (seed, calibration, per‑answer consistency) are applied by the orchestrator — consistency also runs as the gate lens.
