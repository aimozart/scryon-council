# Scryon Council: specification (v1)

## Roles (public handles; never personal or crew names)
| Handle | Does | May not |
|---|---|---|
| `scheduler-agent` | Opens and closes sessions; records no-quorum | Decide anything |
| `orchestrator-agent` | Compiles the packet; recommends | Approve its own work |
| `critic-agent` | Files findings citing evidence | Share a model family with the decision it reviews |
| `contestor-agent` | Argues against the findings | Share a model family with the critic |
| `observer-agent` | Advisory AI review of public bundles | Be treated as proof |
| `human-principal` | Decides each item, last | (hard-rule items are his alone) |

## Event: `council.step`
Closed vocabularies only. Fields: tenant, actor handle, step label, decision, reason, a **subject hash**, a timestamp, the signer's key id and an optional model record (provider, model id, token counts, cost). The reviewed text is hashed together with the session id and **dropped**; it is never stored. Text shaped like a valid identifier is refused.

| Label | Meaning |
|---|---|
| `council_session` | A session was opened, closed, or failed its quorum |
| `council_packet` | The review packet was compiled |
| `council_finding` | The critic filed a finding |
| `council_rebuttal` | The contestor argued against a finding |
| `council_decision` | The Human Principal decided an item |

Decisions: `allow` (accepted), `deny` (rejected), `hold` (deferred or no quorum), `info` (recorded). Reasons: `council_accepted`, `council_rejected`, `council_deferred`, `council_no_quorum`, `none`.

Subject hash: `BLAKE3("entropa-council-subject-v1" 0x00 || frame(text) || frame(session_id))`, where `frame(x)` is the 8-byte big-endian length followed by the bytes. Events are signed ML-DSA-65 and appended to a Merkle log with signed checkpoints; independent witnesses may co-sign checkpoints.

## Quorum
1. **To meet:** critic report + contestor rebuttal + orchestrator recommendation, each signed by its own role.
2. **To decide:** the Human Principal, always last.
3. **Independence:** the reviewer's model family differs from the family of the decision reviewed, and critic and contestor differ from each other.
4. **If not met:** a `council_session` event with reason `council_no_quorum` is recorded, items stay pending, nothing changes.

## Cadence
Per-decision automatic checks (no model); optional daily digest; **weekly** council cycle; **triggered** sessions (override spike, failed policy, incident, hard-rule request); **monthly** retrospective including each reviewer's track record. Members work asynchronously; the Principal's decision is the last signed step.

## Safeguards
Proposes, never changes production (findings become red tests, policy changes through an approvals gate, or lessons). Nobody approves their own work. Every step is a signed record. Reviewers are tested with planted bad decisions. Retrieved precedents are evidence, not authority. Synthetic data only.

## Verify it yourself
Download the bundle from the explorer. The browser verifier checks every event signature, leaf hash, Merkle root, checkpoint signature and witness statement using only the public key.
