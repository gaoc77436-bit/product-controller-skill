# Controller Succession Design

## Purpose

Extend `product-controller` with a safe, user-approved way to prepare a controller handoff, create a fresh Codex task when the runtime supports it, verify that the successor has reconstructed decision-critical facts, and retire the predecessor without overlapping controller or executor ownership.

The external `handoff` skill is an optional document provider. It does not own transfer timing, task creation, successor verification, or controller authority.

## Confirmed behavior

- A request to **prepare** handoff authorizes read-only inspection and a draft in the OS temporary directory only. It does not authorize creating a task, dispatching work, ending an executor, or starting the next batch.
- A second explicit user confirmation authorizes at-most-once successor task creation and onboarding messages. It does not authorize product implementation.
- The successor performs onboarding only and returns `ONBOARDING_READY` or `ONBOARDING_BLOCKED`.
- `ONBOARDING_READY` is not implementation authority. The successor waits for the user to say `continue` in the new task.
- The predecessor is not automatically archived. After verified transfer it stops dispatching and reports the new task to the user.
- Use the same host and saved project by default. Show that target in the confirmation request. A different host, project, environment, or inaccessible temporary file requires a new decision or a safe manual fallback.

## Provider selection

1. If the user explicitly invoked an available `handoff` skill, use it to generate the temporary source-linked draft.
2. Otherwise use the equivalent handoff contract already owned by `product-controller`.
3. Missing `handoff` is not a blocker. Do not change its explicit-only invocation policy and do not claim it was invoked implicitly.

Both providers produce the same controller-owned transfer contract. Product repository status files are prohibited.

## Transfer lifecycle

`ACTIVE_CONTROLLER → PREPARING → DRAFT_READY → USER_APPROVED → SUCCESSOR_ONBOARDING → SUCCESSOR_READY → TRANSFERRED → PREDECESSOR_RETIRED`

Failure states are explicit:

- `HANDOFF_UNSAFE`: an active writer cannot be fenced, the task base is unknown, or material state is unstable.
- `DRAFT_STALE`: semantic source, HEAD, dirty state, executor ownership, or stakeholder feedback changed after drafting.
- `SUCCESSOR_CREATE_UNAVAILABLE`: the runtime cannot create a task; return a manual startup prompt.
- `SUCCESSOR_CREATE_UNCERTAIN`: creation may have queued or succeeded; do not retry until existing tasks are checked by handoff identity/title.
- `ONBOARDING_BLOCKED`: the successor found a factual conflict or cannot access a required source.
- `TRANSFER_BLOCKED`: successor readiness cannot be verified or predecessor ownership cannot be retired safely.

## Safety gates

### Draft gate

The draft records a unique `handoff_id`, source task/thread, focus, current task and semantic lineage, stakeholder outcome, stage/north star, current project/cwd/branch/HEAD/task base/dirty state, assignment and writer ownership, evidence levels, invalidated conclusions, closed routes/failure counts, next action, stop conditions, suggested skills, references, and validity conditions. It has a SHA-256 digest.

Existing specifications, plans, decisions, commits, diffs, tests, and reports are referenced by absolute path/URL and revision/hash instead of copied.

### Creation gate

Immediately before task creation, recompute the draft validity fields. Any material change invalidates the draft and requires a replacement or explicit source-linked delta before creation.

Do not transfer control while a collaboration subagent or other writer remains active. Such executors are scoped to the predecessor task and are not assumed manageable by the successor.

### Successor gate

The initial successor prompt is onboarding-only. It includes the handoff path/hash, source task identity, expected host/project/cwd, and required checks. The successor independently verifies:

1. accessible semantic source and active version/lineage;
2. actual cwd/project, branch, HEAD/task base, and dirty ownership;
3. accepted capabilities and separate implementation/product-path/user-acceptance levels;
4. invalidated conclusions and latest stakeholder feedback;
5. active/retired assignments and absence of an overlapping writer;
6. next action, non-goals, stop condition, and suggested skills.

A bare “read and ready” is not readiness. A conflict produces `ONBOARDING_BLOCKED` and no activation.

### At-most-once creation

If task creation returns a real thread ID, wait for onboarding with that ID. If it returns only a queued/client identifier or an uncertain result, do not pass it to tools requiring a thread ID and do not create a duplicate. Report queued/uncertain state or reconcile via the task list before one safe continuation.

The predecessor may send at most one onboarding correction containing missing source links or facts. A second failure becomes `TRANSFER_BLOCKED`.

## Capability fallback

- No task-creation capability: produce the verified handoff plus an exact manual new-task prompt.
- Temporary file unavailable to the chosen host: do not create a successor there; select the same host or require the user to provide an accessible durable source.
- Non-Git work: replace branch/HEAD fields with immutable input/artifact identity and current mutation ownership.
- Mixed dirty worktree: preserve user WIP and apply the existing dirty-baseline contract; unknown ownership blocks transfer activation.

## Reporting

Before user confirmation, report the proposed successor title, project/host/environment, handoff path/hash, transfer readiness, unresolved risks, and shortest confirmation phrase.

After onboarding, report `READY` or `BLOCKED`, the new task identity, what was independently verified, what remains unproved, and the next user action. Do not equate successful onboarding with product completion.

## Non-goals

- transferring hidden model memory;
- transferring a live subagent between root task trees;
- modifying the external `handoff` skill or its invocation policy;
- auto-archiving the predecessor;
- automatically starting implementation after onboarding;
- duplicating project strategy into a new repository status document;
- guaranteeing permission enforcement beyond behavioral guidance.

## Release strategy

Develop on `feature/v0.3-controller-succession` from the verified v0.2 candidate. Keep the installed `v0.1.0-beta.1`, GitHub `main`, and the external `handoff` skill unchanged until separate installation and publication approval.
