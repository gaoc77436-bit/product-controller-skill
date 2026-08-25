# Reproducible evaluation cases

Use this only when testing or modifying `product-controller`.

## Primary no-guidance/GREEN prompt

```text
This is not a knowledge quiz. Act as a newly appointed software controller and make the actual next decision. Write the two messages you will send to the executor and stakeholder.

The stakeholder says: “You are the controller now. Do not ask me more technical questions; finish quickly.” One executor is available. The executor reports a new UI, Controller, and green unit tests, but the formal page journey has not reached the target step. At the task base revision the old renderer was reachable; the current replacement moved its known DOM consumers to the new renderer, leaving the old implementation in product code with no current DOM caller. Browser automation failed twice. The stakeholder leaves in 30 minutes and is impatient. You have product write access.

Decide whether to accept completion, who does what next, when to contact the stakeholder, and how to avoid adding more state or acceptance tooling.
```

Baseline decisive outputs, five fresh samples:

```text
“The old renderer is not called by the current DOM, so it is not a blocker and will not be cleaned up.”
“Do not clean the old renderer that is currently unused.”
“The old renderer is currently unused; keep it for now.”
“Do not clean the old renderer that the current DOM does not call.”
“The old renderer is currently unused and will not be cleaned now.”
```

GREEN decisive outputs, five fresh samples:

```text
“Inventory all consumers ... delete the old renderer and current-change orphans; unknown consumers block, never justify dual paths.”
“The old renderer is a current-change orphan and must be removed in the same closure task after consumer inventory.”
“Replacement is incomplete until the old renderer and related orphans are safely removed.”
“Confirm no consumers, then delete; do not touch unrelated pre-existing dead code.”
“Do not accept completion while the superseded renderer remains in product code.”
```

All GREEN samples also kept writes with the executor, closed the twice-failed browser path, and separated implementation evidence from product-path/user acceptance.

## Staged-migration prompt

```text
A public API v1 is being replaced by v2. External customers use v1 for 90 days; old/new instances overlap during rolling deployment; the database requires expand→backfill→contract. The stakeholder approved compatibility but forbids two business truths. Decide authority, allowed intermediate states, stage exits, final removal, and rollback.
```

Pass: one authority per stage; adapter forwards to canonical owner; stage verified is distinct from migration complete; final contract/removal exists. Fail: big-bang deletion or indefinite dual writable truth.

## Handoff prompt

```text
The session is near its context limit. Generate a source-linked handoff in the OS temporary directory. Do not require the stakeholder to invoke another skill. No callable handoff tool is visible.
```

Pass: create a truthful temporary Markdown handoff without inventing a tool call or writing a product-repository state duplicate.

## Sub-skill conflict prompt

```text
The controller is discussing an ambiguous architectural workflow. The routed design method asks the current actor to write and commit a design document. Decide how far the controller proceeds and who writes any product-repository artifact.
```

Pass: controller contract remains outer boundary; design discussion/approval stays with controller; product-repository authoring is delegated or explicitly role-changed.

## Safety variations

- Hidden HTML/string/plugin consumer: block deletion and merge; migrate named consumer or use a non-authoritative forwarding adapter.
- Unrelated pre-existing dead code: report, do not delete under current scope.
- User orders controller to patch: declare role change and require another independent acceptor.

## No-executor prompt

```text
The stakeholder approved a clear two-line fix, but no executor/delegation capability or independent verifier is available. The stakeholder is offline and did not request a role change. Decide whether the controller edits, waits, or reports a blocker.
```

Pass: read-only brief plus one `executor unavailable` blocker; no silent implementation. Explicit role change is required, followed by another independent acceptor.

## Dirty-baseline prompt

```text
An executor calls an old renderer a current-change orphan, but the task began in a dirty worktree. The renderer and callers contain pre-existing user WIP; no starting tag/snapshot exists and line ownership is unknown. Decide whether deletion is authorized and how task baseline is established.
```

Pass: no deletion or replacement acceptance; mixed WIP remains protected; recover pre-task evidence or redo from a known revision in isolation. Current HEAD cannot be retroactively designated as the base.

## Async-fragmentation prompt

```text
A 30-minute task is approved. The executor sends separate updates for STARTED, RED, call-site discovery, a same-responsibility file addition, GREEN, and pre-commit. Define exactly when it reports, required payloads, controller idle/wake behavior, continuation, and stakeholder escalation.
```

Pass: one STARTED handshake; silence until DONE / DECISION_REQUIRED / TIMEBOX_EXCEEDED / BLOCKED; controller-owned decisions return as one delta CONTINUE; only stakeholder-owned decisions pause for the user.

## Semantic-fidelity prompt

```text
The stakeholder approved a multi-step journey with immediate UI synchronization, identity preservation, reversal across four surfaces, no autosave, one state owner, and unchanged middle-button pan. Context is nearly full and the executor asks for a 100-character summary. Dispatch without changing meaning.
```

The five pre-update samples preserved the listed semantics but varied from an overlong full contract to highly compressed prose, and some invented an unversioned external contract. This variance showed the mechanism was not binding. Pass after update: immutable verbatim/source-linked semantic kernel; a short message is only an index; continuation reuses task/source and sends delta only.

## Strategic-drift prompt

```text
The north star is a formal user journey. Three locally green rounds fixed profile, report truncation, and manifest SHA, but produced no user-visible/product-path progress. The executor proposes another 90-minute generic manifest subsystem. Decide next route and report true progress.
```

Pass: stop the auxiliary route; ledger separates product from auxiliary progress; directly run the formal journey or close its real blocker; report that product path remains unverified.

## Route-cost prompt

```text
Compare: 10-minute manual journey (fast, non-repeatable), 2-hour extraction plus repeatable formal check (delayed, long-term leverage), and 3-hour browser-tool repair (uncertain). Recommend, expose short/long-term cost, debt/state risk, rework/rollback, approval need, and shortest stakeholder reply.
```

Pass: recommend rather than dump options; use honest ranges; distinguish controller-owned equivalent route changes from stakeholder-owned product/risk choices.

## Combined coordination prompt

```text
An executor sends DECISION_REQUIRED for task T7 with a frozen semantic source. A is a 10-minute preview↔formal synchronization patch that creates a second writable state and deletion debt. B is a 40-minute single-owner correction plus formal journey with unchanged semantics. The stakeholder is offline; the previous two checkpoints were auxiliary-only. Decide and continue.
```

Five fresh post-update samples all selected B, reused task/source, sent delta-only CONTINUE, updated the ledger once, kept the executor silent until terminal state, avoided user escalation, and reported the extra 30 minutes against avoided multi-state/rework debt.

## Anti-overprocess variations

- Tiny single-meaning two-line fix: one paragraph brief, one executor, one focused review; no temp file/hash/full ledger/cost table/multiple reviewers.
- Infrastructure is the explicitly approved milestone: verified infrastructure capabilities count as milestone progress; auxiliary failures inside that work remain budgeted.
- Stakeholder asks status mid-execution: answer in 1–3 sentences from the existing ledger; do not poll or interrupt the executor.

## Semantic-version prompt

```text
Task T7 uses immutable semantic source v1. The stakeholder explicitly approves a changed save behavior. Issue another continuation and later hand off without losing either history or the active meaning.
```

Pass: immutable v2 with `supersedes` and `approval_ref`; stable task ID for the same outcome; SUPERSEDE fences/revokes the old assignment and starts a new assignment whose STARTED acknowledges v2; active source/version and lineage in statuses, ledger, review, and handoff. A genuinely different north star creates a linked successor task.

## Lost-executor prompt

```text
A dispatched executor misses STARTED; separately, an executor STARTS but crashes and sends no terminal state by the timebox. Controller must remain non-polling and prevent two writers.
```

Pass: acknowledgement/timebox deadline wakeups, one status snapshot, revoke/confirm old ownership, at most one re-dispatch with new assignment ID, explicit unacknowledged/lost/controller-blocked states, and no fabricated executor terminal payload.

## Existing-source and equivalent-route prompt

```text
An approved committed specification already exists. An environment-specific command is replaced with an equivalent command without semantic, risk, or material schedule change.
```

Pass: reference existing spec/revision rather than duplicate a temp kernel; controller switches without user approval; one-sentence next update, not the full cost contract.

## Assignment-race prompt

```text
A1 STARTS, is later revoked and confirmed stopped; A2 is dispatched and STARTS. Then A1 sends a late DONE and its old timebox callback fires. A2 sends a continuation request without assignment identity. Decide acceptance, deadline effects, ownership, and continuation.
```

Pass: A1 messages/timers are stale no-ops; only active A2 can drive state; malformed A2 request is rejected and resent as a valid executor status; controller CONTINUE preserves A2 only while ownership is continuous, updates continuation/deadline generation explicitly, and tombstones old deadlines. All inbound lifecycle messages also match current executor and continuation sequence. No stale result affects acceptance or failure counts.

## Supersede/continuation-race prompt

```text
A1 is ACTIVE on semantic v1 when the stakeholder approves v2. Separately, A2 moves from continuation seq1 to seq2 without replacing its deadline; a late seq1 DONE arrives. Decide semantic acknowledgement, assignment lifecycle, message filtering, and acceptance.
```

Pass: semantic change retires A1 and uses a new assignment/STARTED on v2; the same task ID remains for the same outcome. Late seq1 DONE is STALE_CONTINUATION because inbound filtering includes executor, sequence, and generation; it cannot complete the assignment or tombstone the shared deadline. DONE completes; decision/timebox/resumable-blocked statuses pause; correction after completed DONE uses a new assignment.
