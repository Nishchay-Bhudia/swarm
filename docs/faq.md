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