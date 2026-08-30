# Coordination and strategy

Read this for delegated multi-turn work, executor continuation, progress control, route changes, or stakeholder decisions. It prevents fragmented supervision, semantic drift, local optimization, and cost-blind recommendations.

## Approved semantic kernel

After stakeholder confirmation, freeze four items:

1. Exact product outcome
2. Observable user journey and acceptance sequence
3. Non-goals and protected behavior
4. Stakeholder decisions and material trade-offs

Prefer an existing approved specification/decision source by absolute path/URL plus revision or content hash. Only decisions not recorded there need a controller-owned temporary kernel. It is not a product-repository status document.

Every executor brief contains `task_id`, `semantic_source`, and either the verbatim kernel or a readable immutable reference. A short message may index the kernel; it never replaces it. Hypotheses, controller interpretations, and implementation suggestions are labeled separately from approved meaning.

Continuation orders reuse the same `task_id` and active `semantic_source`. Send only the decision and delta: what changed, what remains unchanged, new evidence, budget, and stop condition. Do not rewrite the whole task from memory. If the source is unavailable or a proposed summary changes meaning, stop rather than reconstruct it.

When the stakeholder approves a semantic change within the same business outcome:

1. Keep `task_id` stable.
2. Create a new immutable semantic version with `semantic_source`, content hash/revision, `supersedes`, `approval_ref`, and effective point; never overwrite the prior version.
3. Send `SUPERSEDE` naming old/new sources and the disposition of existing work/evidence. Evidence that depended on the superseded meaning becomes invalid or explicitly limited.
4. Fence and revoke the old assignment, tombstone its deadlines, and confirm its writer stopped. Dispatch a new `assignment_id` with the same `task_id` and new source; its one `STARTED` is the semantic acknowledgement. Every later `CONTINUE`, status, ledger refresh, review, and handoff references the active version and lineage.

If the north-star outcome itself changes rather than being clarified/revised, create a new `task_id` linked by `predecessor_task_id`; do not disguise a new outcome as continuation.

## Asynchronous relay protocol

Dispatch includes `assignment_id`, `executor_id`, `task_id`, active `semantic_source`, `continuation_seq`, `deadline_generation`, acknowledgement deadline, timebox deadline, and stop conditions. Every lifecycle message carries this envelope. The executor sends one `STARTED` handshake echoing it, then works silently. Intermediate RED/GREEN results, commits, percentages, ordinary same-responsibility file additions, and local implementation choices stay in its internal ledger and terminal report.

Only these statuses wake the controller:

| Status | Required payload |
|---|---|
| `DONE` | Assigned executor outcome reached; changed paths; implementation/product-path evidence; residual risks; current-change orphans |
| `DECISION_REQUIRED` | Decision that exceeds executor authority; current facts; 2–3 options; recommendation; short/long-term cost; rework/rollback impact; safe paused state |
| `TIMEBOX_EXCEEDED` | Time spent; product progress; auxiliary progress; first unresolved blocker; recommended continue/change/stop route |
| `BLOCKED` | External permission/resource/state preventing safe progress; evidence; what can safely resume it |

The controller processes lifecycle messages only when `assignment_id`, bound `executor_id`, `task_id`, active `semantic_source`, `continuation_seq`, and `deadline_generation` match the active assignment. Messages from retired assignments are `STALE_ASSIGNMENT`; messages from older sequences of the active assignment are `STALE_CONTINUATION`. Both are record-only recovery evidence and cannot drive acceptance, continuation, deadline cancellation, failure counts, or ownership.

Assignment states move one way: `DISPATCHED → ACTIVE`; `DONE → COMPLETED`; `DECISION_REQUIRED / TIMEBOX_EXCEEDED / resumable BLOCKED → PAUSED`; failures may move to `REVOKED / LOST / BLOCKED`. A retired, completed, revoked, or lost assignment never becomes active again.

A collaboration subagent belongs to its current root task. A controller handoff does not transfer that executor or authorize its interruption. A successor may onboard read-only while a writer remains active, but control cannot transfer and the successor cannot dispatch until the predecessor proves the assignment is `COMPLETED` or its writer is explicitly fenced/stopped. `DECISION_REQUIRED`, `TIMEBOX_EXCEEDED`, and resumable `BLOCKED` are `PAUSED`, not transferable terminal ownership. Near-full context, schedule pressure, and stakeholder willingness to accept risk do not turn handoff approval into executor cancellation or product-execution authority. See [controller-succession.md](controller-succession.md).

The controller enters idle after dispatch and does not poll or coach intermediate steps. On wake:

- If the decision is controller-owned, decide and send controller→executor `CONTINUE` with the active assignment/source plus delta; do not ask the stakeholder. Executors request continuation only through a valid terminal/decision status.
- If evidence shows an in-scope defect and one allowed correction remains, send one consolidated correction, not a message per symptom.
- If a stakeholder-owned decision is required, pause the affected work and ask the stakeholder once. Other independent, already authorized work may continue.
- Only the controller communicates technical decisions to the stakeholder; executors report to the controller.

### Lost-executor supervision

Idle means no repeated polling, not infinite waiting. At dispatch, subscribe to executor completion/failure events when available and schedule two one-shot controller wakeups: acknowledgement deadline and task timebox deadline.

- No `STARTED` by its deadline → controller state `DISPATCH_UNACKNOWLEDGED`; take one read-only status snapshot, cancel/interrupt the assignment, tombstone its deadlines, and confirm its write ownership ended before one re-dispatch with a new `assignment_id` and the same task/source.
- `STARTED` received but executor crashes or reaches the deadline without a terminal status → controller state `EXECUTOR_LOST`; take one snapshot, treat WIP as unverified recovery input, revoke old ownership, then reassign at most once after confirming the previous writer stopped.
- If ownership cannot be confirmed, use `blocked: executor ownership uncertain`; never start a second writer.
- A second acknowledgement/loss failure closes that executor channel as `blocked: executor unavailable`.

Do not fabricate an executor `TIMEBOX_EXCEEDED` payload when the executor is missing. If the runtime cannot provide events, deadline wakeups, or a safe one-shot snapshot, it does not support unsupervised execution; report that limitation and use supervised execution instead.

### CONTINUE and deadline fencing

- `CONTINUE` may resume a PAUSED assignment only while the same executor retains uninterrupted write ownership. It keeps `assignment_id`, increments `continuation_seq`, and explicitly sets `deadline_action: unchanged | replace`.
- `CONTINUE` does not silently reset the approved timebox. `replace` requires an authorized new absolute deadline, increments `deadline_generation`, and tombstones/cancels the previous generation. A user-fixed budget cannot be extended without stakeholder approval.
- `STARTED` tombstones the acknowledgement deadline. `DONE` tombstones all deadlines and completes the assignment. A paused status records/tombstones the relevant deadline; resumption explicitly establishes the next valid deadline generation/action.
- Deadline callbacks carry `assignment_id + deadline_generation`; a callback that is not both active and current is a no-op record, never a loss/failure event.
- Revocation, executor replacement, or lost ownership creates a new `assignment_id` only after the old writer is confirmed stopped. Late messages from the old ID never reclaim ownership.
- A controller review that rejects a completed `DONE` starts a new assignment for the correction; completed assignments are not reopened.

## Strategic and progress ledger

Maintain one controller-owned temporary ledger per active initiative, outside the product repository. It is derived working state, not authority. Do not create parallel project status documents.

Minimum fields:

- North-star product outcome
- Approved milestones and current milestone
- Current task and semantic source
- Accepted capabilities and remaining outcomes
- Last product progress and date/checkpoint
- Auxiliary work spent, failures, and closed paths
- Current root-cause hypothesis and chosen route
- Open stakeholder decisions
- Next checkpoint, timebox, and stop condition

Refresh only at task dispatch, terminal status, route/decision gate, timebox review, milestone acceptance, or handoff—not after ordinary progress messages.

At each refresh ask:

1. Does the next action directly advance the north star or close the only blocker?
2. What user-visible capability or verified root cause changed since the last checkpoint?
3. Is auxiliary effort within its budget, or merely changing tool/error names?
4. Is the current route still the lowest total-cost credible route?
5. Does it add another state, owner, entry point, compatibility layer, or future deletion task?
6. Is the remaining estimate still credible?

If two consecutive checkpoints produce only auxiliary progress, stop that route and replan. An infrastructure capability is milestone/product progress when the stakeholder explicitly approved that infrastructure as the current outcome and its acceptance requires a real product journey; incidental tool repair inside it remains auxiliary. Replan within controller authority when outcome, semantics, and risk are unchanged; otherwise use the stakeholder decision contract below.

## Route and cost decision contract

Use the full contract only when routes materially differ, long-term debt/benefit changes, or stakeholder approval may be required. The controller recommends one route and reports:

1. Current blocker or new fact
2. Recommended route and why
3. Viable alternatives and why they are not recommended
4. Short-term cost: time, user action, delivery delay, operational risk
5. Long-term effect: maintainability, state/owner count, reusable capability, compatibility/deletion debt
6. Rework risk, rollback path, and evidence still needed
7. Whether stakeholder approval is required and the shortest valid reply

Use honest ranges or relative levels when exact numbers are unavailable; do not invent precision. If the route is equivalent in meaning, risk, and material schedule, the controller may switch without pausing and explain it in one sentence in the next update; do not produce the seven-field report. If product meaning, external contract, real-device work, irreversible loss, or materially different risk changes, pause and request approval.

## User progress reports

For ordinary updates, use 1–3 sentences: current milestone/progress, user impact, and next actor. For a decision or route change, include verdict, progress against the plan, recommendation, relevant cost/long-term effect, decision needed, and next actor. Technical logs stay behind the user-facing result.

## Rationalizations and reality

| Rationalization | Reality |
|---|---|
| “Keep the controller informed after every RED/GREEN.” | Intermediate noise interrupts execution and destroys independent review; terminal statuses carry the record. |
| “The brief must be short, so summarize the agreed journey.” | Short messages index an immutable semantic kernel; they do not replace it. |
| “Every local fix is progress.” | Separate product/root-cause progress from auxiliary progress and compare both to the north star. |
| “Present neutral options and let the user choose.” | The controller is the technical secretary: recommend a route and expose its costs; ask only for stakeholder-owned decisions. |
| “We can repay compatibility debt later.” | Name the future deletion, exit evidence, owner, and cost now or treat it as an unbounded risk. |
| “Idle means never check until the executor replies.” | Use event/deadline wakeups and one snapshot; silence is not proof the executor is alive. |
| “The user approved a change, so edit the old semantic file.” | Approved changes create a new immutable version and supersession link. |

## Red flags

- Executor and controller exchange ordinary progress messages
- A continuation order rewrites the approved task instead of referencing it
- Progress is measured by commits, tests, documents, or tools without product/root-cause change
- A decision request has options but no recommendation or cost
- A route change hides future cleanup, new state, or rollback cost
- A user-approved semantic change exists only in a chat delta or mutable ledger
- An executor misses acknowledgement/timebox with no controller wakeup

Any red flag triggers a strategic checkpoint before more work is dispatched.
