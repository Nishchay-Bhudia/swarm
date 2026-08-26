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

**Architect (independent):** confirmed the monolith's actual bottleneck, from the
codebase, is the payment-confirmation step blocking the whole request thread on a slow
third-party call — not a general capacity problem across the whole service.

**Debate:** PM's "weeks not quarters" requirement directly conflicts with a full service
split, which the Architect estimates at months. Architect proposed the actual fix only
needs to extract the payment-confirmation step into its own async-processed component —
a much smaller, targeted split, not a full microservices rewrite. PM accepted this as
satisfying the real requirement at a fraction of the cost.

**Decision logged:** "Scope is a targeted extraction of the payment-confirmation path
only, not a full service decomposition. Rationale: the actual bottleneck is one blocking
call, not general load; a full rewrite would cost months for a problem solvable in weeks.
Raised by: Architect, accepted by PM as satisfying the real requirement."

## Design phase

**Architect:** proposed extracting payment confirmation into a separately-deployed
async worker, communicating via a message queue, with the main order service polling or
receiving a callback on completion.

**Security (independent, threat-modeling the problem space before seeing the specific
design):** flagged that moving payment confirmation to an async boundary introduces a
new question — what happens between "order placed" and "payment confirmed" if the async
worker never responds (crashes, queue message lost), specifically whether the order can
be shipped/fulfilled in that gap before confirmation lands. That's a new trust boundary
that didn't exist when it was one blocking synchronous call.

**Debate:** Architect's initial design didn't specify how the main service prevents
fulfillment before confirmation. Conceded this was a real gap — not a hypothetical,
since it's a straightforward race in the new async design — and added an explicit
"pending confirmation" order state that blocks fulfillment until the async worker
confirms or the request times out and falls back to synchronous retry.

**Critical Security finding, binding per the veto hierarchy:** the message queue must
not carry raw payment tokens in plaintext between services — flagged as Critical, blocks
finalization until addressed. Architect updated the design to pass only an opaque
payment-intent reference, with the actual token handled solely by the existing
PCI-scoped payment component, unchanged.

## Review phase

Security re-reviewed the updated design: no plaintext token exposure in the queue
message, "pending confirmation" state correctly blocks premature fulfillment. Finding
closed. QA reviewed the async design for the queue-message-lost case: confirmed the
timeout-fallback-to-synchronous-retry path directly covers it. No blocking findings
remained.

## Final decision log (excerpt)
1. Scope narrowed from full service split to targeted payment-confirmation extraction —
   Architect, driven by PM's timeline requirement.
2. "Pending confirmation" order state added to prevent premature fulfillment during the
   new async gap — Security-raised gap in the initial design, fixed same phase.
3. Payment tokens never traverse the message queue in plaintext — Security critical
   finding, binding, fixed before finalization.
4. Full microservices decomposition explicitly deferred, not rejected — logged in
   `risks` as a future option if load grows beyond what the targeted fix handles.
