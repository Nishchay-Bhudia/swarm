# Workflow: Review

Roles involved: **Security** (if present — runs first and in parallel with QA, never
skipped for the Security-critical tier), **QA**, **Architect** (if present — reviews
final shape against the design decisions), **Tech Lead** (arbitrates and closes).

## Goal
Leave this phase with `security_findings` and `qa_findings` at zero unresolved blocking
items (per the termination condition in `SKILL.md`), and a closed decision log.

## Steps

1. **Independent review, in parallel.** Security reviews the actual diff/output against
   the checklist in `roles/security-engineer.md` (input validation, authn/authz, secrets
   exposure, trust boundary crossings, dependency surface). QA reviews the actual
   diff/output against its checklist, independent of what it flagged during
   Implementation — things look different once code exists versus once a plan existed.
   Architect (if present) reviews whether the final shape still matches the decisions
   logged during Design, or drifted during implementation.
2. **Cross-exposure and structured debate (max rounds per tier cap):** each reviewing
   role states position → evidence (concrete line/behavior, not vague concern) →
   severity → recommendation. Developer responds to each: fix, or argue scope/severity
   per that role's dedicated pushback (Developer can contest QA severity and Security
   proportionality, never contest a Security critical finding's validity without
   Tech Lead sign-off).
3. **Apply the veto hierarchy from `roles/tech-lead.md`** for anything contested.
   Security criticals are binding by default. Everything else, Tech Lead arbitrates and
   logs why.
4. **Mandatory disagreement check** applies per-role as in other phases — a reviewing
   role that finds nothing must state what it specifically checked, not just "LGTM."

## Exit condition — this gates the whole run's termination (see `SKILL.md` Step 4)
`security_findings` and `qa_findings` both have zero unresolved blocking items. If a hard
round cap is hit first, Tech Lead makes the call, logs the open item in
`open_disagreements`, and the run reports it as unresolved rather than pretending it
closed clean.
