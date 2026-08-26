# Failure Modes

Swarm's structure exists specifically to prevent these. If you're watching a run and

see one of these happening, it means the structure broke down somewhere — not that the
approach is fundamentally flawed. Each entry names the signal and the fix.


| Failure mode | Detection signal | Fix |
|---|---|---|
| **Echo chamber / consensus collapse** | Every role proposes the same thing in round 1, no objections | Independent analysis wasn't actually isolated — a role's output leaked into another's framing before it should have. Redo the analysis step per-role, from that role's objective file alone. |

| **Fake disagreement / theater** | A role objects, then immediately concedes with no new evidence ("fair point, never mind") | Only treat an objection as resolved when the objecting role names what specifically changed its mind. A concession with no new evidence is theater, not resolution — push back. |
| **Conformity bias** | One role's position dominates regardless of the strength of counter-argument | Score on whether each role actually engaged with the opposing evidence, not on whether they agreed. The Tech Lead should be able to state the losing argument's strongest point even while overruling it. |
| **Over-deliberation drift** | Round count climbing, no field in the state object is actually changing | This is the smart-stop signal — arbitrate now using the veto hierarchy, even under the hard round cap. More rounds without new evidence is waste, not rigor. |

| **Context rot mid-run** | The swarm's own output contradicts an earlier logged decision | Re-read the state object (not the conversation transcript) and re-assert the contradicted decision before continuing. This is why the state object exists instead of relying on scrollback. |
| **Role drift** | A role's output stops sounding like its stated objective (e.g., "Security" starts commenting on code style) | Re-read that role's file before its next turn. Role definitions lose weight the longer a run goes without reinforcement — reinject them at phase boundaries, which `SKILL.md` already schedules. |
| **Scope creep mid-swarm** | Implementation plan or code has grown past what Requirements confirmed | PM has scope veto for exactly this. Flag it, get explicit sign-off logged in the decision log, or cut it back. |

| **Over-engineering** | Architect's design is materially more complex than the requirements need | This is the Architect role's own characteristic failure mode. PM or Tech Lead should call it out directly — that's a legitimate check, not something to smooth over. |
| **Missing edge cases that pass review anyway** | Implementation passes QA's stated checks but the checklist itself was shallow | QA's checklist in `roles/qa-engineer.md` is concrete on purpose (boundary values, concurrency, dependency failure, malformed input, partial state, backward compatibility) — a review that didn't visibly walk that list wasn't a real review. |
| **Silent finding drop** | A Security or QA finding is raised once and never appears in the final decision log | Every raised finding must end up in `qa_findings`/`security_findings` as either fixed, or explicitly deferred with a named role's sign-off in `risks`. If it just vanishes, that's the bug. |

| **Token/round runaway** | The run keeps going well past the tier's expected cost with no termination in sight | The hard caps in `SKILL.md` Step 4 exist precisely for this — iteration count and the AND-condition on findings. If you hit the cap, report what's unresolved rather than continuing past it. |

## Why these specific fixes, not generic ones

Generic instructions like "please consider multiple perspectives" or "review thoroughly"
don't prevent these failures — they're the exact prompts that produce fake diversity.
What actually works, per the design behind this skill: independent analysis before

cross-exposure, mandatory concrete disagreement (not optional), role-specific checklists
instead of open-ended review, and hard stop conditions instead of "keep going until
satisfied." If a run is misbehaving, the fix is almost always "which of these four things