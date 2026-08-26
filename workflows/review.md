# Workflow: Review

Roles involved: **Security** (if present — runs first and in parallel with QA, never
skipped for the Security-critical tier), **QA**, **Architect** (if present — reviews
final shape against the design decisions), **Tech Lead** (arbitrates and closes).

## Goal
Leave this phase with `security_findings` and `qa_findings` at zero unresolved blocking
items (per the termination condition in `SKILL.md`), and a closed decision log.