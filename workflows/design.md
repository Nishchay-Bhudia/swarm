# Workflow: Design

Roles involved: **Architect** (owns this phase), **Product Manager** (scope check),
**Security Engineer** (if present in the tier — threat model this phase's proposal
before implementation exists, not after).

## Goal
Leave this phase with `architectural_decisions` populated — each with a rationale and
which role raised it — and, if Security is present, an early threat model that shapes
the design rather than a bolt-on review after code exists.

## Steps

1. **Independent analysis.** Architect proposes a design in isolation, sourced from
   `requirements` and `constraints` from the prior phase. If Security is present, they
   independently threat-model the *proposed problem space* (not the Architect's specific
   design yet — work from the requirements) and list the trust boundaries this kind of
   change typically has.
2. **Cross-exposure.** Reveal the Architect's design and Security's threat model to each
   other and to PM.
3. **Structured debate (max rounds per tier cap):**
   - Architect: position → evidence (why this design fits the constraints) → concern
     about anything Security or PM raised that seems to conflict → recommendation.
   - Security (if present): position (what this design does well/poorly against the
     threat model) → evidence (concrete exploit scenario for any gap) → recommendation.
     Security's critical findings here are binding per the veto hierarchy in
     `roles/tech-lead.md` — a critical gap blocks moving to implementation, it does not
     just get logged as a risk.
   - PM: concern only if the design has grown past what requirements confirmed — invoke
     scope veto if so.
4. **Mandatory disagreement check.** Architect and Security must each raise at least one
   concrete objection to the other's framing, or explicitly state why none applies.
5. **Close the phase.** Write each real decision to `architectural_decisions` with its
   rationale and origin. Do not log trivial choices (variable naming) — only the ones
   that would need a "why did we do it this way" answer later.

## Exit condition
The design does not have an open critical Security finding, does not visibly exceed
`requirements` scope without a logged PM sign-off, and `architectural_decisions` has
concrete rationale entries a future engineer could actually use.
