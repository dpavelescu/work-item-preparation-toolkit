# Work‑Item Preparation Toolkit

**Turn raw requirements into a clear user story — or clarify a story you already have — before planning and implementation, with a human in the loop.**

One capability, **`prepare-work-item`**, with two modes that share the same clarification engine:

- **author** — input is raw requirements (formal, a rough draft, or free text) and there's no authoritative story yet → it **writes the user story**.
- **clarify** — a story already exists and is the source of truth → it emits a **Clarified Work‑Item Spec** that *references* the story and never modifies it.

Both focus on the **business** side — functional and non‑functional requirements with end‑user relevance — and produce an artifact that downstream planning/implementation **builds against**. They do **not** design the solution.

---

## The problem

Work items reach planning and implementation ambiguous: the user value is implied, non‑functional expectations are unstated, scope edges are blurry, technical assumptions are hidden, and the scope is sometimes too big to review. Whoever picks it up — AI or human — **fills the gaps with assumptions**, which means misinterpretation, rework, and scope creep.

---

## What this does

Before downstream work, `prepare-work-item`:
- detects what's **missing or ambiguous** against a consistent **clarification checklist**;
- **clarifies it with you interactively — one question at a time, most critical first**;
- **captures** the result as either a **user story** (author) or a **Clarified Work‑Item Spec** (clarify) — a contract for the next phase.

It keeps the work **end‑user oriented**, sized to **~a few days**, with **explicit in/out scope**, and **defers technical detail to the next phase** — never inventing requirements the input and your answers don't support.

---

## The rules that keep it safe

*Everything below relies on these.*

- **Business, not technical.** Clarify *what / why / for whom*; defer the *how*.
- **End‑user first.** A guard flags work that looks technical — unless it's an **explicit enabler**, then it's handled in enabler mode.
- **One blocking question at a time, most critical first.** Ask only what genuinely needs a human, not the obvious; technical questions are deferred, not asked.
- **Resolve the important questions before producing the artifact.** The output is complete and built from the answers — never a partial artifact with a pile of open points.
- **Don't invent.** Only what the input + answers support; inferred items are marked *assumed — confirm*.
- **Stay consistent.** A later clarification can't silently overwrite or contradict an earlier one, and the assembled draft is checked for internal contradictions before it's captured.
- **Source of truth.** In *clarify* mode the story stays authoritative (referenced, never copied); in *author* mode the drafted story is a **draft pending your approval**.
- **Humans approve.** This makes what reaches planning safer; it doesn't replace review.

---

## How it works: one small loop

```
input (requirements OR a story)
   └─ prepare-work-item
        detect gaps  →  clarify with you (one question at a time)  →  capture the artifact
                                            (gated on the clarification checklist + a consistency pass)
   ▼
planning / implementation  — build to the artifact; resolve the deferred technical items here
```

---

## What's in this repo

Two idiomatic builds — same behavior, packaged for each tool. Use the one for your agent.

```
README.md
Work-Item-Preparation-Playbook.md          ← the full reference (tool-neutral)

.claude/                                    ← Claude Code build
  skills/   prepare-work-item               (orchestrator = skill)
            clarification-checklist | prepared-work-item-spec   (shared helpers)
  agents/   functional-clarity-reviewer | nfr-experience-reviewer
            scope-sizing-reviewer | fit-sources-reviewer   (4 detect lenses)
            consistency-reviewer            (gate lens)     (lenses = sub-agents)

.github/                                    ← GitHub Copilot build
  agents/   prepare-work-item               (orchestrator = custom agent; delegates via agents:)
            + the 5 lenses                  (4 detect + 1 gate; lenses = custom agents)
  skills/   clarification-checklist | prepared-work-item-spec   (shared helpers)
```

> Same responsibilities and boundaries in both; only the packaging differs (Claude Code: skills + sub-agents; Copilot: custom agents + Agent Skills). Both builds extract the two helper skills (kept byte-identical); the orchestrator and lenses differ only by harness. Pick one build.

---

## Onboarding

1. **Copy the build for your tool** into your project (`.claude/` for Claude Code, `.github/agents/` + `.github/skills/` for Copilot).
2. **Run it** on your input — it infers the mode and tells you which it's in:
   - *author:* point it at your requirements/draft.
   - *clarify:* point it at the existing story.
3. **Answer its questions** (one at a time, most critical first).
4. **Review and approve** the drafted story / Clarified Work‑Item Spec — fix anything wrong, resolve the minor `TBD`s.
5. **Hand it to planning/implementation** — they build to it and resolve the deferred technical items.

---

## When *not* to use this

- A trivial, unambiguous change.
- A pure technical task with no decision — unless it's an **explicit enabler** (then it's in scope).
- As a replacement for human product/business judgment — it surfaces and structures; you decide.

---

## Learn more

The [Work‑Item Preparation Playbook](Work-Item-Preparation-Playbook.md) — the tool‑neutral reference: the checklist, the guards, the process, and the output contract.
