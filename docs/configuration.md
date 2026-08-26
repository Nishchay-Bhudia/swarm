# Configuration

Swarm has no config file — it's a skill made of plain markdown, so every override
below is done by saying it in the same message that invokes the skill. These are
per-invocation, not persistent.

## Force a specific team
By default, team size is chosen by the complexity routing table in `SKILL.md` Step 0.
Override it directly:

> "Run this through Swarm with just Developer + QA, skip PM."

> "Run this through Swarm with the full 6-role team even though it's a small change."

The skill will use your explicit team instead of auto-classifying — it still enforces
independent analysis and mandatory disagreement for whichever roles you picked.

## Force the Security-critical override off
The routing table auto-escalates anything touching auth/crypto/payments/PII to the
5-role security team, non-negotiable by default. If you're certain a match is a false
positive (e.g., the word "password" appears in a comment, not in actual auth logic), say
so explicitly:

> "This isn't actually security-relevant — the file just has 'auth' in its name, it's a
> UI label. Route as Medium."

The skill will still spot-check that claim rather than blindly trusting it, per the
Security role's mandate — expect one clarifying pass, not an automatic override.

## Adjust round caps
Default max debate rounds scale with tier (1 for Tiny up to 5 for Security-critical, per
the table in `SKILL.md`). Override for a single run:

> "Cap every phase at 2 rounds even though this is a Large task — I want speed over
> exhaustiveness here."

## Skip the swarm entirely
If you invoke Swarm on something Tiny/Small, the skill already tells you it's
skipping the swarm and does the work directly — you don't need to configure this away,
it's the default behavior for low-stakes tasks.

## Running review-only (no implementation)
Swarm can run just the review phase against existing code instead of building
something new:

> "Run Swarm's review workflow against this diff — Security + QA + Architect, no
> implementation phase, I already wrote the code."

This uses `workflows/review.md` directly against your existing diff instead of code the
swarm produced itself.
