# FAQ

**Is this just Claude roleplaying six characters?**
No — that's the thing it's specifically designed not to be. Personas alone collapse into
agreement; this skill forces independent analysis before roles see each other's output,
mandates a concrete disagreement per phase, uses role-specific checklists instead of
"review thoroughly," and arbitrates through a fixed veto hierarchy instead of a vote. The
roleplay framing is the interface; the state machine and the conflicting objectives are
the substance.

**Will this replace my test suite / static analysis / linter?**
No. Swarm finds bugs, missing edge cases, and design issues through structured
review — it doesn't execute anything. It's a complement to testing and automated
analysis, not a replacement. QA's findings should turn into real tests, not stand in for
them.

**Why does it cost more tokens than just asking Claude directly?**
Because it's doing more real work — five roles' worth of independent analysis, cross-
review, and arbitration, not five paraphrases of one answer. Expect roughly 2–4x the
tokens of a direct prompt, scaled by tier. That cost is deliberately not paid on
Tiny/Small tasks — the skill skips the swarm and answers directly for those, see Step 0
in `SKILL.md`.

**Why does it sometimes tell me to skip the swarm?**
Because for a one-line fix or a typo, five roles debating adds latency and cost with no
quality benefit — there's no genuine ambiguity or trade-off for conflicting objectives to
surface. Forcing structure onto a task that doesn't need it is exactly the kind of
overhead this skill is designed to avoid, not produce.

**Can I add a 7th role, like DevOps or UX?**
Not by default — the routing table and role files only cover the six defined in this
repo, matched to research on what produces genuine axiom-level conflict in software
engineering decisions. If your task genuinely needs a role this skill doesn't define,
say so explicitly and Swarm will proceed without inventing an undefined role rather
than faking one. Contributions adding a well-specified new role (see the shape of the
existing files in `roles/`) are welcome.

**What if two roles never reach agreement?**
That's expected and handled, not a bug. The Tech Lead arbitrates via the veto hierarchy
in `roles/tech-lead.md` — nobody votes, consensus isn't the goal. If a decision is still
open at the round cap, it's logged as an open disagreement in the final output rather
than silently resolved or endlessly debated.

**Does the Security role ever get overruled?**
Only on paper, and only by the Tech Lead writing an explicit risk-acceptance into the
decision log with a stated reason — never silently, and never by Developer/PM pushback
alone. That's a deliberate asymmetry: every other role's veto can be argued down through
the hierarchy, Security's critical findings require an explicit, attributable override.

**Can I use this for code review only, without generating new code?**
Yes — see the "review-only" invocation pattern in `docs/configuration.md`. It runs
Security + QA + Architect against an existing diff instead of code the swarm wrote
itself.

**How do I know it's not just producing more confident-sounding output without more
actual quality?**
Check the decision log Swarm always produces on finalization (`SKILL.md` Step 5) —
it names every real decision, who raised it, what was rejected, and why. If that log
reads as generic ("team agreed this was a good approach") rather than specific
(concrete objections, named edge cases, real severity calls), the run didn't actually
follow the structure and should be treated skeptically regardless of the final answer's
polish.
