# Workflow: Intake


Not a phase most tiers need as a separate step — Step 0 in `SKILL.md` covers
classification. This file exists for the one case that needs more than the routing

table: **ambiguous task descriptions**, where the complexity tier itself is unclear.


## When the task is ambiguous
If you cannot tell from the task description whether this is Small/Medium/Large, or

whether it touches a security boundary, do not guess silently and do not launch a
5-role swarm on a speculative reading. Instead:


1. State your best-guess classification and why, in one line.

2. If the guess materially changes team size (e.g., Small vs. Large) or triggers the
   Security-critical override, ask the user one direct question to resolve it rather

   than proceeding on an assumption that could 4x the cost of the wrong call.
3. If the ambiguity doesn't change team size or routing (e.g., it's clearly Medium

   either way), proceed without asking — don't stall on cosmetic uncertainty.
