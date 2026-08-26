# Example: Feature Request (Medium tier)

**Task given to Swarm:** "Add a 'export my data' button to the account settings page
that lets users download their data as a file."

**Classification:** Medium — multi-file (UI + backend endpoint), some ambiguity on
scope and format. Team: Product Manager, Developer, QA, Tech Lead.

---

## Requirements phase

**PM (independent):** Restated need — "users want a copy of their own data, likely for
portability or peace of mind." Smallest version: a button that generates a JSON export
of the user's own profile + content, downloadable synchronously if small, emailed as a
link if large. Explicitly out of scope: scheduled/recurring exports, exporting other
users' data, CSV/PDF format variants — v1 is JSON only.

**Developer (independent, works from `requirements` once PM closes — not run in
isolation from PM the way Architect vs. PM would be in a Large-tier design phase):**
flagged that "the user's data" isn't defined precisely enough to implement — does it
include deleted-but-retained records, third-party-linked account data, or just what's
directly editable in settings?

**Debate (round 1):** PM concedes the definition gap is real, narrows scope explicitly:
v1 exports only data visible/editable in the user's own account (profile + their own
created content), not deleted records or third-party linkages — those become a logged
future scope item, not silently dropped.

**Decision logged:** "Export scope = directly-owned, currently-visible data only, JSON
format, synchronous for small accounts / async email-link for large. Rationale: matches
actual portability need without taking on deleted-record retention policy questions
that need separate legal/product input. Raised by: PM, refined after Developer's scope
gap."

## Implementation phase

**Developer:** implements the endpoint + button, synchronous path only initially.

**QA (independent, from requirements):** edge cases raised — account with zero content
(empty export shouldn't error), account large enough to need the async path (what's the
actual size threshold, is it defined anywhere), concurrent export requests from the same
user (should the second request queue or reject), and what happens if the download is
interrupted mid-transfer.

**Debate:** Developer had not implemented the async path or a size threshold at all —
conceded this was a real gap, not an edge case to defer, since PM's requirements
explicitly included the async path. Zero-content case: agreed to fix, one-line guard.
Concurrent requests: Developer argued low real-world likelihood for this specific
feature and proposed simply not de-duplicating for v1, logged as accepted risk rather
than fixed, since duplicate exports are wasteful but not harmful. QA accepted that
resolution given the stated reasoning.

## Review phase

QA re-checked the fixed implementation against its checklist; no new blocking findings.
Tech Lead closed the run.

## Final decision log (excerpt)
1. Export scope narrowed to owned/visible data only — PM, refined by Developer's gap.
2. Async path for large accounts is in-scope, was initially missed by Developer,
   fixed after QA caught it against PM's original requirements — not treated as a new
   feature request.
3. Concurrent duplicate exports: accepted as low-risk, unfixed in v1 — Developer's
   proposal, QA sign-off logged.
4. Deleted-record and third-party-linked data: explicitly out of scope for this run,
   flagged in `risks` for future product/legal scoping.
