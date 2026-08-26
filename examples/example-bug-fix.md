# Example: Bug Fix (Small tier)

**Task given to Swarm:** "Users report the search page sometimes shows stale results
after they change a filter. Fix it."

**Classification:** Small — single-page bug, but root cause and correct fix aren't
obvious from the report alone, so it's not Tiny. Team: Developer, QA, Tech Lead.

---

## Implementation phase

**Developer (independent):** traced it to a race condition — the search request from
the *previous* filter state can resolve after the request from the *new* filter state if
the network timing lines up, and the UI naively renders whichever response arrives last
rather than the one matching the current filter. Proposed fix: tag each request with a
request ID, only render the response if its ID matches the latest request issued.

**QA (independent, from the bug report alone, not from the Developer's diagnosis):**
listed the edge cases a "sometimes stale" report implies: rapid successive filter
changes (more than two in flight), a request that never resolves at all (does the UI
get stuck, separate from the staleness bug), and whether this same class of bug exists
anywhere else results get rendered from an async call (pagination, sort order).

**Debate:** Developer's request-ID fix directly covers the rapid-successive-changes
case (each new request invalidates all prior IDs, not just the immediately previous
one). The never-resolves case was a real gap Developer hadn't considered — conceded and
added a timeout fallback. On "does this bug class exist elsewhere": Developer checked
and confirmed pagination has the same unguarded pattern, but argued fixing it now is
scope creep beyond the reported bug. QA agreed the diagnosis was right but flagged it as
a real risk rather than dropping it silently.

## Review phase (compressed, 2-round cap for Small tier)

QA confirmed the request-ID fix and timeout fallback both work against its listed edge
cases via the reasoning above (no code execution in this skill — findings are structural,
translate to a real test before shipping). No blocking findings remained.

## Final decision log (excerpt)
1. Root cause: unguarded async response race, fixed via request-ID tagging — Developer.
2. Added timeout fallback for never-resolving requests — QA-raised gap, fixed same pass
   since it's the same code path already being touched, not separate scope.
3. Pagination has the identical unguarded-race pattern — explicitly flagged in `risks`
   as a known, deliberately out-of-scope follow-up, not silently noticed and dropped.
