---
name: clarification-checklist
description: >-
  The consistent checklist for clarifying a work item's business requirements — what must be
  clarified and when it's clarified enough to capture. Use while detecting gaps or gating the
  output in prepare-work-item (author or clarify mode).
---

*Derived copy — canonical source is `Work-Item-Preparation-Playbook.md`; if they disagree, the playbook wins.*

Assess the work item against these — checking what's **missing** as much as what's present.
Each item is **Met / Deferred / Open**; it scopes the clarification and signals when it's
*clarified enough* to capture. (Means, not the deliverable.)

1. **User & value** — persona(s) and the outcome are explicit.
2. **Functional behavior** — observable behaviors as scenarios (happy path + meaningful alternates + user-visible error/edge cases).
3. **Non-functional (UX-relevant)** — only NFRs that affect the end-user experience, each a *business expectation* (measurable where possible): responsiveness/perceived performance, availability-as-felt, accessibility, localization, privacy & data handling/consent, security-as-felt, compliance, auditability.
4. **Scope boundaries** — explicit in-scope and out-of-scope (out-of-scope stated, never inferred).
5. **Acceptance criteria** — testable, business-level (Given/When/Then).
6. **Assumptions & dependencies** — business-level, named (incl. cross-team/other-item deps).
7. **Success signals** — how we'd know it delivered value, where applicable.
8. **Sizing** — fits ~a few days; if not, a split is proposed.
9. **Reference sources** — linked sources considered; any additional needed source named (provided or flagged).
10. **No unresolved blocking ambiguity** — every business-critical question answered, deferred, or out-of-scope.
11. **Work type** — user-facing (default) or explicitly an enabler.

**Gate:** capture the artifact only when every item is Met or explicitly Deferred/Out-of-scope
**and the assembled draft is internally consistent** (no contradiction across requirements, ACs,
NFRs, or scope); otherwise the run is Not ready (a resumable clarification agenda). A missing
**load-bearing** item is blocking; a minor one is a deferred TBD. Stay business-level — defer technical detail.
