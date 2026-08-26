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