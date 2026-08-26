# Failure Modes

Swarm's structure exists specifically to prevent these. If you're watching a run and

see one of these happening, it means the structure broke down somewhere — not that the
approach is fundamentally flawed. Each entry names the signal and the fix.


| Failure mode | Detection signal | Fix |
|---|---|---|
| **Echo chamber / consensus collapse** | Every role proposes the same thing in round 1, no objections | Independent analysis wasn't actually isolated — a role's output leaked into another's framing before it should have. Redo the analysis step per-role, from that role's objective file alone. |