---
name: swarm
description: Simulates a multi-role engineering team (PM, Architect, Developer, QA, Security, Tech Lead) with genuinely conflicting objectives to plan, design, or review code. Use for architecture decisions, feature planning, scope negotiation, security-sensitive changes, or any task where a single perspective is likely to miss edge cases, over-engineer, or accept scope creep. Not for typos, one-line fixes, or pure boilerplate — use a direct prompt for those.
---

# Swarm

Swarm is not a roleplay skill. It is a **state machine** that runs a task through
several roles with axiom-level conflicting objectives, forces each role to commit to an
independent position before seeing anyone else's, and then arbitrates. The value comes
from structure, not personas — a role told to "be the QA engineer" without a conflicting
objective, an isolation requirement, and a mandatory-disagreement rule will just agree
with whatever came first. This file enforces the structure. Read it in full before acting;
it fits in one context load by design.

## When to use this skill vs. a direct prompt

Use Swarm when the task is an **architecture decision, feature plan, security-relevant
change, refactor with unclear scope, or anything where a wrong early assumption is
expensive to unwind**. Skip it — and just answer directly — for typos, one-line fixes,
pure formatting/boilerplate, or anything where only one kind of judgment applies. Swarm
costs 2–4x the tokens and several minutes of wall-clock versus a direct answer; that cost
must be justified by the decision's stakes. If you're unsure, ask the user in one line
rather than defaulting to the swarm.

## Step 0 — Classify complexity and select the team

Score the task (rough heuristic, not arithmetic precision):

- Touches one file, behavior is obvious, no ambiguity → **Tiny**
- Single-file bug fix or small addition, low ambiguity → **Small**
- Multi-file feature, some requirements ambiguity → **Medium**
- New subsystem, module, or service → **Large**
- Changes auth, crypto, payments, PII handling, or trust boundaries → **Security-critical**
  (this flag overrides the size-based tier — always route to the 5-role security team)
- Irreversible or hard-to-unwind technical/product decision → **Architectural**

Route the team and workflow: