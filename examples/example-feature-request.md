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