# Scryon Council

A **decision review council** for recorded AI decisions: independent validators, human and AI, review what was recorded, argue against their own conclusions, and sign every step into a tamper-evident ledger. This repository is the public **specification and documentation**; the production code stays private. All data is synthetic.

- Info page: https://entropa.space/scryon-council
- Explorer (verify it yourself in the browser): https://scryon-council.entropa.space
- Built on [Entropa](https://github.com/aimozart/entropa-public), the signed Merkle ledger.

## In one paragraph
Entropa records AI decisions. The council asks the next question: were they good? A scheduler opens a session, the orchestrator compiles a packet, a **critic** from another model provider files findings, a **contestor** argues the other side, and the **Human Principal** decides, last. Each step is a hash-only, signed record. The council only proposes; it never changes production.

## The quorum, briefly
A session can decide only if the critic's report, the contestor's rebuttal and the orchestrator's recommendation are all on the record. Otherwise items stay pending and a signed **no-quorum** event records it: no quorum, no change. See [SPEC.md](SPEC.md).

## Status (2026-11-05)
| Piece | Status |
|---|---|
| Council ledger on the node, signed with a managed cloud key | Built, running |
| Public explorer with in-browser verification | Built, live |
| First session recorded: **no quorum**, because the critic and contestor are not connected yet | Done (the correct outcome) |
| Critic and contestor model runs (different providers) | Next |
| Human Principal's private review view | Planned |
| Decision memory (precedents as evidence, never authority) | Planned |

## Honest limits
The ledger proves recorded steps are unaltered; it cannot prove nothing was omitted. One human operates the whole crew, so this is technical separation of duties, not organisational independence. Aligned with and mapped to AI-governance practice; not assessed, not certified.

Licence: MIT. Security: see [SECURITY.md](SECURITY.md).
