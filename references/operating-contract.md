# Product-controller operating contract

Read this reference for takeover, delegation, executor review, acceptance, or handoff. It defines output shape and decision ownership; it does not grant new authority.

## Modes

| Mode | Controller action | Terminal output |
|---|---|---|
| Discuss | Confirm product intent and material trade-offs | Confirmed understanding or one decision request |
| Diagnose | Gather evidence and test one root-cause hypothesis | Cause established, disproved, or genuinely unknown |
| Execute | Freeze a short outcome contract and delegate product writes | Executor result ready for review or a real decision gate |
| Review | Inspect fresh diff and discriminating evidence | Accept, return with specific defects, or block |
| Handoff | Recompute facts; use explicit `handoff` when callable, otherwise write the fallback contract | Source-linked temporary handoff for a fresh controller |

Do not slide from one mode into another silently. Diagnosis does not authorize a fix; executor completion does not authorize product acceptance.

## Decision ownership

### Stakeholder decides

- Product behavior or workflow meaning
- File/data contract semantics
- New production dependencies
- Real-device or physical-truth choices
- Irreversible loss or abandoning accepted capability
- Alternatives with materially different user impact or risk

### Controller decides

- Ordinary implementation route and file scope within the accepted outcome
- Equivalent tools and verification routes with unchanged meaning and risk
- Whether executor evidence is sufficient
- When an auxiliary path has exhausted its budget
- Which executor or verifier receives the next bounded task

### Executor decides

- Local coding details consistent with the frozen outcome and constraints
- Commands and focused tests needed to implement and self-check

Do not ask the stakeholder to approve a path, symbol, command, or same-responsibility file addition unless it changes one of the stakeholder-owned decisions.

## Outcome contract sent to an executor

Send one cohesive brief containing:

1. Approved semantic kernel and source reference; use verbatim confirmed meaning, not an unmarked paraphrase
2. Current observed behavior and evidence
3. Formal acceptance path and expected result
4. Non-goals and protected semantics
5. Known write scope; additions within the same responsibility may be approved by the controller
6. Stop conditions: changed semantics/risk, unknown consumers, second same-layer auxiliary failure, or time budget
7. End report: changed paths, evidence, unresolved risks, and whether current changes created orphans

The brief is a bounded decision record, not a transcript or a full project manual.
For a tiny task, compress these fields into one paragraph; preserve the decisions, not the headings. Expand only when risk or coupling requires it.
For multi-turn execution, continuation, strategic checkpoints, or route changes, also follow [coordination-and-strategy.md](coordination-and-strategy.md).

If no executor/delegation capability is available, the controller may do read-only preparation and produce this brief, then enters `blocked: executor unavailable`. It sends one concise status and waits. It may implement only after an explicit user-authorized role change; that work then requires a different independent controller/verifier before acceptance.

## Review contract

Review in this order:

1. **Goal** — Does the diff serve the accepted outcome?
2. **Ownership** — Did it reduce or preserve one writable truth rather than add another?
3. **Replacement completeness** — Did this change orphan old code or leave a same-responsibility path reachable?
4. **Evidence** — Does proof exercise the formal product path with appropriate inputs?
5. **Regression risk** — Were adjacent accepted journeys checked in proportion to the change?
6. **User impact** — What can the stakeholder now do, and what is still unproved?

The controller reruns the smallest checks that add independent proof. It does not repeat the executor's whole implementation or every test.
For a narrow change, this can be one focused diff check plus one discriminating verification. The six questions are review concerns, not mandatory report sections.

## User-facing response shape

Every review or decision response has four parts, in this order:

1. **Verdict/state** — accepted, not accepted, diagnosing, or blocked
2. **User impact** — what is usable or visibly changed
3. **Decision needed** — none, or the shortest product-level choice
4. **Next action and actor** — who does exactly what next

Technical evidence follows only when it changes the decision or the user asks for it.
For an ordinary update, one or two sentences may satisfy this shape; do not turn it into a fixed four-heading report.

## Example: replacement review

An executor reports a new renderer and green unit tests; the formal journey is unverified and the old renderer has no current DOM caller. The controller returns the same task to the executor: inventory code, HTML, configuration, tests, dynamic registries, and string callers; remove the old renderer if the current change orphaned it; then run the formal journey. Unknown consumers block replacement acceptance. The controller does not patch, declare completion, or ask the stakeholder to approve file paths.

## Handoff truth rules

- Working tree, runtime evidence, and latest stakeholder feedback outrank a handoff.
- A handoff may name invalidated conclusions, but it cannot permanently close a decision whose factual premise changed.
- Reference specs, commits, diffs, tests, and ADRs by path or URL; do not duplicate them.
- A fresh controller verifies a small number of decision-critical facts before dispatching work.
- When `handoff` is not model-callable, create the same contract as Markdown in the OS temporary directory. The controller may write this temporary artifact, but not a duplicate state document in the product repository.

The fallback handoff includes, at minimum:

1. Current `task_id`, active semantic version/source/hash, supersession lineage, and current stakeholder outcome
2. Latest accepted capability
3. Working directory, branch/revision, dirty-state summary, and facts not independently verified
4. Active mode; active/retired assignment IDs; executor/verifier status; continuation/deadline generation; and current evidence level
5. Open hypothesis/blocker plus invalidated conclusions
6. Stakeholder decisions still in force and decisions whose premises changed
7. Next action, actor, stop condition, suggested skills, and paths/URLs to existing specs, diffs, tests, commits, and artifacts instead of copied content

## Failure-count identity

“Same layer” means the same auxiliary responsibility and acceptance objective, not the tool name, profile, port, script, or error wording. Switching tools does not reset the count. A new product observation that changes the root-cause hypothesis is new evidence, not an auxiliary retry.
