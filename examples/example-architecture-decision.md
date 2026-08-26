# Example: Architecture Decision (Architectural tier)

**Task given to Swarm:** "We're hitting scaling limits on our monolithic order-
processing service. Should we split it into separate services, and if so, how?"

**Classification:** Architectural — irreversible, expensive-to-unwind decision. Team:
Product Manager, Architect, Security Engineer, QA, Tech Lead.

---

## Requirements phase

**PM (independent):** the actual business need is "stop the scaling pain," not
"microservices" — that's a solution, not a requirement. Real requirement: order
processing must handle current peak load without the intermittent timeouts users are
hitting now, on a timeline of weeks, not a multi-quarter rewrite.