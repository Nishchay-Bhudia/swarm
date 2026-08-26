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