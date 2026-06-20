# Work‑Item Preparation — Playbook

**Audience:** Product owners, business analysts, tech leads, developers, QA.

> **Canonical source.** This playbook is the single source of truth for the **clarification checklist**, the **guards**, and the **Prepared Work‑Item Spec** structure. The per‑tool copies (`.claude/skills/`, `.claude/agents/`, `.github/skills/`, `.github/agents/`) are derived for packaging — when any of them disagrees with this file, **this file wins** and the copy is the bug. Update here first, then propagate.

---

## Tool scope & terms

This playbook is the **tool‑neutral reference** — it describes the capability by what it *does*: one **orchestrator** (`prepare-work-item`), four **reviewer lenses**, and two shared **helpers** (a clarification checklist and the output spec). Packaging differs per coding agent:

- **Claude Code** — the orchestrator is a *skill* (`.claude/skills/`), the lenses are *sub-agents* (`.claude/agents/`); invoked as a `/slash` command.
- **GitHub Copilot** — all five are *custom agents* (`.github/agents/`) plus two *Agent Skills* (`.github/skills/`); invoked from the agent picker.

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
6. **Gate, don't dump.** Produce the artifact only when the checklist is satisfied (or items are explicitly deferred/out‑of‑scope) — never a partial artifact with a list of open points.
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
- **Source guard.** If clarifying a business‑critical point needs a missing/unlinked source, make it a clarification item (a missing load‑bearing source is blocking).
- **Conflict guard.** If the input contradicts itself or a known source, raise it — don't resolve it silently.

---

## The process (detect → clarify → gate)

1. **Determine mode** (author vs clarify) and state it. **Read the input and follow its linked sources;** identify additional sources needed. Run the **technical‑work guard** (classify user‑facing vs enabler).
2. **Detect** against the checklist — check what's **missing** as much as what's present. Delegate the lenses (parallel) for a rich item; **apply their criteria inline for a small/clear one** (right‑size). Findings: gaps, ambiguity (>1 reading), missing NFRs, boundary blur, oversize, missing sources, conflicts — with candidate questions.
3. **Clarify (human‑in‑the‑loop).** Ask **one business‑level question at a time, most critical first, until no blocking item remains** (use the IDE's native prompt if available; ask only what genuinely needs a human). **Defer technical questions.** You may draft on the fly.
4. **Gate & capture.**
   - Checklist Met/Deferred/Out‑of‑scope → produce the artifact (author: the **user story**; clarify: the **Clarified Work‑Item Spec**) as a **draft pending approval**.
   - Otherwise → **Not ready**: an ordered, resumable clarification agenda; **no artifact**.

---

## The output: the Prepared Work‑Item Spec

A predefined structure with **two roles**: in *author* mode it leads with the **story narrative** and **is** the work item; in *clarify* mode it **references** the external story and is the companion contract. It never copies the source.

```markdown
# <work-item title>
Mode: author | clarify    Status: Captured | Captured with deferrals | Not ready
Work type: User-facing | Enabler    Checklist: <n>/11    Source of truth: <this (draft) | link to story>

## Story            (author mode — the narrative)
As a <persona>, I want <capability>, so that <outcome>.

## Reference sources  (read these — pointers, not copied)
- <linked source> — why relevant   · Needed but missing: <source> — what it resolves

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

**Traceability:** each functional/NFR/AC item is tagged with its origin — a requirement, a story section, or the **human answer** that resolved it (or *assumed — confirm*) — so downstream knows what's authoritative vs clarified vs assumed.

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
- **Lenses** (4 **sub‑agents**, read‑only, parallel, flat) — independent, non‑interactive analyses that each find gaps in one dimension and return findings; they fire selectively (inline for small items). They map to the checklist:
  - `functional-clarity-reviewer` — user/value, behaviors, alternate/error paths, acceptance criteria.
  - `nfr-experience-reviewer` — UX‑relevant NFRs that are missing or vague.
  - `scope-sizing-reviewer` — boundary blur (in/out), ~few‑days sizing/split, dependencies.
  - `fit-sources-reviewer` — end‑user/technical‑work guard + enabler, linked + missing sources, conflicts.
- **Helpers** (2 **skills**, reused verbatim) — `clarification-checklist` (the measure) and `prepared-work-item-spec` (the output structure). *(Claude inlines these in the orchestrator/lenses; Copilot extracts them as Agent Skills.)*

The interactive clarification protocol stays **in the orchestrator** (single, interactive driver — not a skill); each guard lives **in its lens**.
