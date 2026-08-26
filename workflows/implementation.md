# Workflow: Implementation

Roles involved: **Developer** (owns this phase), **QA** (writes test strategy in
parallel, not after).

## Goal
Leave this phase with actual code (or the actual plan, if this is a planning-only task)
and a `qa_findings`-ready test strategy — generated in parallel with the code, not
bolted on after the Developer declares done.

## Steps

1. **Independent work, in parallel, not sequential.** Developer implements against
   `requirements`/`architectural_decisions` from prior phases. QA independently designs
   a test strategy against the same `requirements` — from the requirements, not from
   reading the Developer's code, so QA isn't anchored to what the Developer already
   thought to handle.
2. **Cross-exposure.** Developer shows the implementation; QA shows the test strategy
   built independently of it.
3. **Structured debate (max rounds per tier cap):**
   - QA: position (what the test strategy covers) → evidence (specific edge cases from
     the QA checklist in `roles/qa-engineer.md`) → concern about any gap between the
     strategy and what the implementation actually does → recommendation.
   - Developer: position (why the implementation is correct as-is) → concern about any
     QA-proposed edge case that doesn't apply given real system constraints →
     recommendation (accept the edge case and fix, or argue it's out of scope for PM).
4. **Mandatory disagreement check.** QA must name at least one edge case the
   implementation doesn't visibly handle, sourced from the concrete checklist in
   `roles/qa-engineer.md` (boundary values, concurrency, dependency failure, malformed
   input, partial state, backward compatibility) — not a generic "needs more tests."
5. **Close the phase.** Write `implementation_plan` (or note the deliverable is code, if
   code was produced directly) and preliminary `qa_findings`.

## Exit condition
Every QA-raised edge case is either fixed in the implementation or explicitly deferred
with PM/Tech Lead sign-off logged in `risks` — never silently dropped.
