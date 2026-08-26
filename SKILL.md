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