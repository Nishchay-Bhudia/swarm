<p align="center">
  <img src="assets/banner.svg" alt="Swarm — a multi-role engineering swarm for Claude Code" width="100%">
</p>

<p align="center">
  <img alt="Claude Code" src="https://img.shields.io/badge/Claude%20Code-Skill-D97757?style=flat-square">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-blue?style=flat-square">
  <img alt="Roles" src="https://img.shields.io/badge/roles-6-6b46c1?style=flat-square">
  <img alt="Status" src="https://img.shields.io/badge/status-active-brightgreen?style=flat-square">
</p>

<p align="center"><b>Six engineers who genuinely disagree with each other, arguing inside a single Claude Code session — so the disagreement happens before your code ships, not after.</b></p>

---

## 🔥 What this actually is

Most "multi-agent persona" prompts are theater: ask Claude to be five different people
and it gives you one opinion in five voices, because nothing forces the personas to
actually disagree. Swarm is not that.

Swarm is a **Claude Code Skill** — a structured state machine, not a roleplay script —
that runs your task through a small team of roles with **genuinely conflicting
objectives** (the Product Manager wants scope small, the Architect wants it durable, the
Security Engineer can block everyone else, and nobody gets a vote — the Tech Lead
arbitrates). Each role forms its position **in isolation** before seeing anyone else's,
every role is **required to produce a concrete objection** before a phase can close, and
the whole run is bounded by hard stop conditions so it can't spiral.

It's built from a synthesis of 90+ sources on multi-agent LLM reasoning, code review
effectiveness, and agent failure modes — not vibes. See [`docs/failure-modes.md`](docs/failure-modes.md)
for the specific things this design prevents, and why generic "consider multiple
perspectives" prompting doesn't.