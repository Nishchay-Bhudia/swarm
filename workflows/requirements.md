# Workflow: Requirements

Roles involved: **Product Manager** (owns this phase), **Architect** (if present in the
tier — constraints only, not scope).

## Goal
Leave this phase with a `requirements` list the PM will defend with a scope veto for
the rest of the run, and a `constraints` list the Architect has flagged as real technical
limits the requirements must respect.

## Steps

1. **Independent analysis.** PM writes, in isolation: the restated problem in one
   sentence, the smallest version that solves it, and what's explicitly out of scope.
   If the Architect is present, they separately write, in isolation: what existing
   interfaces/data model/constraints this touches, without reading the PM's scope draft
   first.
2. **Cross-exposure.** Reveal both to each other.
3. **Structured debate (max rounds per the tier's cap in SKILL.md):**
   - PM: position (proposed scope) → evidence (why this solves the real need) → concern
     about any constraint the Architect raised that seems to be gold-plating → recommendation.
   - Architect (if present): position (constraints that must be respected) → evidence
     (what breaks if ignored) → concern about any scope item that seems to ignore a real
     constraint → recommendation.
4. **Mandatory disagreement check.** If both sides simply agreed on round 1 with no
   objection raised, that's the echo-chamber failure mode — redo the isolation step
   before proceeding; do not accept silent first-round consensus as a real result.
5. **Close the phase.** Write final `requirements` and `constraints` into the state
   object. PM's scope veto is now active: any implementation work outside `requirements`
   from here on must come back to PM for explicit sign-off, logged as a decision.