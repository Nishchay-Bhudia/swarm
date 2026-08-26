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