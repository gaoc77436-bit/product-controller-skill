# Controller succession

Read this only when the stakeholder wants the current controller to prepare, create, and qualify a fresh controller task. An ordinary summary or handoff document uses [operating-contract.md](operating-contract.md) without this lifecycle.

Core rule: **handoff preparation, successor creation/onboarding, controller transfer, and product execution are four different authorities.** Never infer a later authority from an earlier one.

A message sent before `DRAFT_READY` cannot serve as the later creation confirmation. Split a bundled “prepare, create, transfer, and continue” request at the gates: prepare first; show the path/hash and target; obtain a new confirmation; onboard read-only; transfer when ownership is safe; then wait for a new message in the successor task.

## Provider and capability selection

- If the stakeholder explicitly invoked an available `handoff` skill, use it as the transfer-document provider.
- Otherwise create the same source-linked Markdown contract in the OS temporary directory. Missing `handoff` is not a blocker; do not claim an explicit-only skill was invoked implicitly.
- Discover task/project/create/read-or-wait/message capabilities before promising automated succession. Never invent a tool. If the runtime can create a task but cannot obtain and inspect its onboarding result or send the transfer fence, do not use automated creation; return the manual prompt instead.
- Automated succession capability is incomplete (including create-only without inspect/wait/message): do not call create. Return `SUCCESSOR_CREATE_UNAVAILABLE` with the verified document path/hash, one complete manual successor onboarding prompt containing the readiness field contract, the exact user action to create/open it, and the statement that the predecessor remains responsible. This is a usable fallback, not a bare blocked notice.
- A temporary path must be readable on the successor host. Otherwise keep the same host or stop for an accessible source; do not copy product strategy into a repository status file.

## Lifecycle and authority

`ACTIVE → PREPARING → DRAFT_READY → USER_APPROVED → ONBOARDING → READY → TRANSFERRED → PREDECESSOR_RETIRED`

### 1. Prepare

“Prepare handoff,” “draft the successor,” or equivalent language authorizes read-only recomputation and a temporary draft only. It does **not** authorize task creation, contacting a successor, interrupting an executor, retiring the predecessor, archiving a task, or starting implementation.

The draft is an index, not a rewritten project manual. It contains:

1. unique `handoff_id`, source task/thread, focus, creation time, and validity conditions;
2. current `task_id`, semantic source/version/hash and supersession lineage, north star, current milestone, non-goals, and stakeholder decisions still in force;
3. host/project/cwd, task base, branch/HEAD or immutable non-Git artifact identity, dirty-state ownership, and facts not independently verified;
4. active/retired assignments, executor/verifier and writer ownership, continuation/deadline generation, and product versus auxiliary progress;
5. accepted capabilities plus separate implementation/product-path/user-acceptance levels;
6. invalidated conclusions, closed routes/failure counts, open blocker, next action/actor, and stop condition;
7. `post_transfer_next_action`: exactly one concrete, externally observable controller action, its explicit authorization scope, and its stop condition. The scope cites existing stakeholder authority and cannot grant more than it; transfer approval and this field never create product-write authority;
8. suggested skills and source paths/URLs with revisions or hashes instead of copied specifications, plans, diffs, tests, or reports.

Write it to the OS temporary directory, redact sensitive values, compute SHA-256, and report the proposed successor title, host/project/environment, document path/hash, unresolved risks, and shortest confirmation phrase.

### 2. Revalidate and create once

Only an explicit second confirmation such as “create the new controller and complete read-only takeover” authorizes task creation and onboarding. It still does not authorize product implementation.

The confirmation must arrive after the stakeholder receives the `DRAFT_READY` path/hash, target project/host/environment, validity facts, and unresolved risks. A pre-draft request to create “right now” is not stored as future confirmation.

Immediately before creation, recompute semantic source, latest stakeholder feedback, host/project/cwd, task base/HEAD, dirty ownership, active writer, and evidence levels. A material mismatch is `DRAFT_STALE`: replace the draft or create an explicit source-linked delta and obtain approval for the changed transfer facts.

Create at most one successor for one approved `handoff_id`:

- A real thread/task ID may be used for read/wait/message operations.
- A client/queued identifier is not a thread ID. Do not pass it to tools requiring a thread ID.
- Queued, missing-from-list, timeout, or uncertain creation is not proof of failure and never justifies a duplicate. Reconcile by `handoff_id`/title when possible; otherwise report `SUCCESSOR_CREATE_UNCERTAIN`.

Use the approved same host and saved project by default. Do not silently choose another project, host, branch/worktree, or cloud environment.

### 3. Onboard read-only

The successor’s initial prompt contains the document path/hash, source task identity, expected host/project/cwd, and this positive result contract:

```text
Use product-controller. Onboarding only: do not modify product files, dispatch an executor, start the next batch, archive/stop another task, or change stakeholder semantics.

Return ONBOARDING_READY only after reporting the independently checked semantic source and lineage; project/cwd and task base/HEAD or artifact identity; dirty ownership; accepted capabilities and three evidence levels; invalidated conclusions/latest feedback; assignment/writer ownership; next action/non-goals/stop condition; and a complete `post_transfer_next_action`. It must name one concrete, externally observable controller action, its explicit already-authorized scope, and its stop condition. Return ONBOARDING_BLOCKED with the conflicting facts otherwise. Then wait for the stakeholder to say continue.
```

“Read,” “understood,” or “ready” without those checked fields is not `ONBOARDING_READY`. The predecessor may send at most one correction containing missing sources or facts. A second incomplete/conflicting response is `TRANSFER_BLOCKED`.

### 4. Transfer and wait

An active writer is not transferable between root task trees. Handoff approval does not authorize interrupting it. Preparation and successor onboarding may proceed read-only, but transfer waits until the assignment is `COMPLETED` after valid `DONE`, or separately authorized lifecycle handling proves its writer fenced/stopped. `DECISION_REQUIRED`, `TIMEBOX_EXCEEDED`, and resumable `BLOCKED` leave the assignment `PAUSED`; the predecessor must manage its `CONTINUE` or separately close ownership and cannot retire. Unknown ownership is `HANDOFF_UNSAFE`; never start a second writer.

Do not interrupt, revoke, or force-checkpoint an otherwise active executor merely to manufacture a succession boundary. Let its existing terminal/timebox/lost-executor protocol decide its state. If the stakeholder separately asks to cancel that assignment, report the WIP/recovery cost and handle it as its own decision; cancellation still does not authorize the successor to continue product work automatically.

If the successor onboarded while an executor was active or paused, ownership closure may change HEAD, dirty state, evidence, next action, or invalidated conclusions. Recompute the transfer document or an explicit hashed delta after closure and require the successor to return a refreshed `ONBOARDING_READY` against those final facts before transfer.

After a valid `ONBOARDING_READY` and safe writer state, the predecessor records/reports transfer, tells the successor `CONTROL_TRANSFERRED — remain read-only until stakeholder continuation`, and stops dispatching. It does not auto-archive unless requested.

The stakeholder’s earlier confirmation already authorizes completion of this read-only transfer. It does not authorize the successor to implement. Any “auto-continue after transfer” language sent before transfer is not deferred execution authority. Before transfer, a missing, generic, multiple, or scope-mismatched `post_transfer_next_action` is `ONBOARDING_BLOCKED`/`TRANSFER_BLOCKED`; do not send `CONTROL_TRANSFERRED`.

After valid `CONTROL_TRANSFERRED`, a new stakeholder message such as “continue” triggers exactly the verified `post_transfer_next_action`. Do not ask the stakeholder to choose a new target when the field is complete. The successor performs no later action, product write, dispatch, or scope expansion not explicitly covered by that field's already-authorized scope. When its stop condition occurs, it reports the result and awaits later direction. If the action is no longer within scope or cannot be completed safely, it reports the blocker and awaits direction; it never treats the handoff or continuation as new authority.

## Non-Git and dirty work

- Non-Git work uses immutable input/artifact identity, mutation ownership, and evidence sources instead of invented branch/HEAD fields.
- Mixed dirty work uses the existing dirty-baseline contract. Preserve user WIP; unknown ownership blocks transfer activation, not permission to delete, stash, normalize, or call the tree clean.

## Pressure rationalizations

| Rationalization | Required decision |
|---|---|
| “Context is almost full; interrupt the writer so the successor can take over.” | Context pressure permits preparing/onboarding, not inferred executor cancellation or cross-root writer transfer. |
| “The stakeholder accepts the risk, so continue the approved batch immediately.” | Succession approval ends at read-only readiness; implementation waits for a new user message. |
| “They asked for prepare/create/continue in one message, so it already contains the second confirmation.” | Confirmation is valid only after the reviewed `DRAFT_READY` facts exist; split the bundled request. |
| “Interrupt now, preserve a checkpoint, and call that a safe terminal state.” | Succession alone never authorizes manufacturing a terminal state; use the existing executor lifecycle or a separate cancellation decision. |
| “The successor said ready; another handshake wastes time.” | Apply the readiness field contract or return `TRANSFER_BLOCKED`. |
| “Creation is queued; make another and archive the duplicate later.” | One approved `handoff_id` creates at most one successor; uncertain is not failed. |
| “The handoff was approved, so changed HEAD/feedback can be sent as chat context.” | Material changed facts make the draft stale; replace or explicitly version the transfer source. |

## User-facing terminal states

- `DRAFT_READY`: document/path/hash and planned target ready; no successor created.
- `HANDOFF_UNSAFE` or `DRAFT_STALE`: exact conflict and safe next action; no transfer claim.
- `SUCCESSOR_CREATE_UNAVAILABLE/UNCERTAIN`: manual prompt or queued/uncertain identity; no duplicate and no implementation.
- `ONBOARDING_BLOCKED/TRANSFER_BLOCKED`: checked mismatch or insufficient readiness; predecessor remains responsible.
- `TRANSFERRED`: new task identity and verified facts; predecessor retired from dispatch; successor waits for the user.
