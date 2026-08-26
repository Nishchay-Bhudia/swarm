<p align="center">
  <img src="assets/banner.svg" alt="Swarm — a multi-role engineering swarm for Claude Code" width="100%">
</p>

<p align="center">
  <img alt="Claude Code" src="https://img.shields.io/badge/Claude%20Code-Skill-D97757?style=flat-square">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-blue?style=flat-square">
  <img alt="Roles" src="https://img.shields.io/badge/roles-6-6b46c1?style=flat-square">
</p>

Six engineering roles with genuinely conflicting priorities, arguing through a task
inside a single Claude Code session, so the disagreements happen before the code ships
instead of after.

---

## What this is

Most "multi-agent persona" prompts don't hold up. Ask Claude to be five different people
and you tend to get one opinion in five voices, because nothing actually forces the
personas to disagree with each other. Swarm tries to avoid that trap.

It's a Claude Code Skill — a state machine, not a script for Claude to roleplay from —
that runs a task through a small team of roles that want different things. The Product
Manager wants scope small. The Architect wants it to hold up in a year. The Security
Engineer can block everyone else and nobody can vote that down. The Tech Lead makes the
final call and has to say why the losing argument lost. Each role writes its position on
its own before it sees anyone else's, and every role has to raise a real objection before
a phase is allowed to close.

None of this is guesswork — it's based on reading through a fair amount of research on
multi-agent LLM reasoning, code review effectiveness, and where these setups tend to
break down. `docs/failure-modes.md` covers the specific failure patterns this design is
meant to catch, and why prompting Claude to just "consider multiple perspectives" doesn't
really do that on its own.

## Why structure instead of just personas

| Without structure | With Swarm |
|---|---|
| Every role sees the same context and lands on the same answer immediately | Each role writes its position in isolation before seeing the others' |
| "Also think about security" — easy to skip, no teeth | Security has a binding veto on critical findings, overridable only by an explicit, logged risk-acceptance from the Tech Lead |
| Roles agree with whoever spoke first | A role has to raise a concrete objection, or explicitly say why it has none |
| Debate runs until it feels finished | Hard round caps and a checkable stop condition — see below |
| You get an answer | You get an answer plus a log of what was rejected and why |

## The team

| Role | Optimizes for | Can veto | Conflicts with |
|---|---|---|---|
| Product Manager | User value, scope, timeline | Scope creep | Architect, Security (on proportionality) |
| Architect | Maintainability, long-term cost | Structural debt | PM (over-scoping), Developer (over-simplifying) |
| Developer | Correct, simple implementation | Designs that aren't actually buildable | Architect, QA, Security (rework cost) |
| QA Engineer | Edge cases, regressions | Untested critical paths | Developer (how much testing is enough) |
| Security Engineer | Vulnerability prevention | Critical findings, absolute | Everyone, on friction vs. actual risk |
| Tech Lead | The right trade-off | Arbitrates the rest | Nobody — arbitrates, doesn't argue its own stake |

Full definitions, including each role's independent-analysis checklist, are in
[`roles/`](roles/).

## How a run flows

```mermaid
flowchart TD
    A[Task in] --> B{Classify complexity}
    B -->|Tiny / Small| Z[Answer directly, swarm skipped]
    B -->|Medium / Large / Architectural / Security-critical| C[Select team]
    C --> D[Requirements]
    D --> E[Design]
    E --> F[Implementation]
    F --> G[Review]
    G --> H{Termination met?}
    H -->|No, under round cap| G
    H -->|Yes, or hard cap hit| I[Decision log + deliverable + open risks]
```

Every phase runs the same loop: independent analysis, then cross-exposure, then
structured debate, then a check that someone actually disagreed, then the Tech Lead
arbitrates. The exact mechanics per phase are in [`workflows/`](workflows/). The routing
table that decides team size and round caps by task complexity is in
[`SKILL.md`](SKILL.md).

## Termination

```
STOP WHEN:
  requirements confirmed AND
  architecture approved   AND
  implementation complete AND
  qa_findings has 0 unresolved blockers AND
  security_findings has 0 unresolved blockers
  OR iteration_count >= 8
  OR roughly 10 minutes of wall-clock work has elapsed
```

If a hard cap is hit before the actual condition is satisfied, Swarm says so and hands
back the open items rather than pretending everything resolved cleanly. Details in
[`SKILL.md`](SKILL.md), step 4.

## Install

Swarm is plain markdown — no build step, no dependencies.

**Clone into your skills directory (all projects):**
```bash
git clone https://github.com/Nishchay-Bhudia/swarm.git ~/.claude/skills/swarm
```

**Or drop it into one project:**
```bash
git clone https://github.com/Nishchay-Bhudia/swarm.git .claude/skills/swarm
```

Claude Code picks up skills from `~/.claude/skills/` or a project's `.claude/skills/`
automatically — nothing else to register. Restart your session if it was already open.

## Using it

Describe the task normally — it classifies complexity and picks its own team:

```
Use Swarm to plan how we should add rate limiting to our public API.
```

```
Run this through Swarm — I want the security and QA perspective on this diff
before I open a PR.
```

For something small, like a one-line fix, Swarm will say it's skipping the team and
just answer directly. That's intended behavior, not a bug — see
[When to use this vs. a direct prompt](SKILL.md#when-to-use-this-skill-vs-a-direct-prompt).

For a specific team, a tighter round cap, or a review-only run against code you already
wrote, see [`docs/configuration.md`](docs/configuration.md).

## Worked examples

Full task-to-decision-log walkthroughs:

- [Feature request](examples/example-feature-request.md) — scope gets narrowed after the Developer catches a gap the PM's requirements missed
- [Bug fix](examples/example-bug-fix.md) — QA catches a second failure mode the Developer's first fix didn't cover
- [Architecture decision](examples/example-architecture-decision.md) — Security's binding veto changes the design before it ships

## When to use this, and when not to

Good fit: architecture decisions, feature scope negotiation, security-relevant changes,
refactors with unclear boundaries, multi-perspective review before opening a PR.

Not a good fit: typos, one-line fixes, boilerplate, anything where only one kind of
judgment actually applies. It runs at roughly 2–4x the token cost of a direct answer and
takes a few extra minutes. That's worth it when a wrong assumption is expensive to
unwind, and wasted otherwise — the skill checks for this itself at step 0 of
[`SKILL.md`](SKILL.md) rather than relying on you to remember to skip it.

## Repository layout

```
swarm/
├── SKILL.md            orchestration: routing, phases, state, termination
├── roles/              one file per role — objective, vetoes, checklist
├── workflows/          phase mechanics: requirements, design, implementation, review
├── examples/           worked walkthroughs
├── docs/
│   ├── configuration.md   overrides: team size, round caps, review-only mode
│   ├── failure-modes.md   what breaks, how to spot it, how this prevents it
│   └── faq.md             common questions
└── assets/             README visuals
```

## FAQ

[`docs/faq.md`](docs/faq.md) covers the recurring questions — is this just roleplay,
does it replace testing, can Security actually be overruled, and a few others.

## Contributing

Issues and PRs are welcome, especially a well-specified new role (follow the shape of
the existing files in `roles/`: an objective, who it genuinely conflicts with and why,
what it can veto, a concrete checklist) or another worked example under `examples/`.

## License

MIT — see [`LICENSE`](LICENSE).
