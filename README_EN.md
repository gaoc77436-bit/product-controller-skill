# Product Controller: a software controller skill for Codex

[简体中文](README.md) | **English**

> Preserve stakeholder intent while coordinating coding agents through complex takeovers, repeated rework, state replacement, and real acceptance.

`product-controller` is not another prompt that asks an agent to “try harder.” It introduces an explicit software controller between a stakeholder and coding agents. The controller owns semantic fidelity, bounded delegation, evidence review, strategic progress, rework control, and durable handoff. Executors own product writes. The stakeholder is involved only when a decision changes product meaning, external risk, or final user acceptance.

**Current maturity: `v0.1.0-beta.1`, recommended only for supervised trials.**

## Why it exists

Complex software work often fails because the collaboration chain drifts, not because the model cannot write code:

```text
Stakeholder's plain-language goal
  ↓ compressed into a subtly different controller summary
Executor solves the nearest local problem
  ↓ new code is layered on while old state remains reachable
Tests follow the easiest passing path
  ↓ technical reports say success while the supported user journey is broken
The next controller inherits stale conclusions
  ↓ rework, state proliferation, and maintenance cost continue to grow
```

This skill targets that failure chain independently of programming language, framework, or test tool.

## Problems it addresses

| Common failure | `product-controller` guardrail |
|---|---|
| Approved meaning changes during relay | A source-linked, versioned semantic kernel; revisions explicitly supersede old versions |
| Controller and executor chatter constantly | One cohesive dispatch, silence after STARTED, wake only at a terminal or real decision gate |
| A local blocker replaces the strategic goal | A lightweight strategy ledger separates product progress from auxiliary progress |
| New and old implementations remain active together | Replacement review inventories old entries, writers, adapters, and current-change orphans |
| Tests prove only the easiest route | Evidence is separated into implementation, supported product path, and user acceptance |
| Technical execution logs are forwarded to the user | Reports explain user impact, recommendation, costs, rework risk, and the next decision |
| Handoffs become a stale source of truth | Current code, runtime evidence, and latest stakeholder feedback outrank handoff notes |
| A retired executor's late result affects new work | Task, executor, semantic source, continuation, and deadline generations fence lifecycle messages |

## Core capabilities

### 1. Explicit decision ownership

- **Stakeholder:** product behavior, data semantics, external dependencies, real devices, irreversible loss, and final experience.
- **Controller:** implementation route within the approved outcome, task scope, executor selection, evidence sufficiency, and rework decisions.
- **Executor:** local coding details and focused self-checks that do not alter product meaning.

Ordinary technical commands do not become repeated user approval gates. A workaround that changes meaning or long-term risk must be explained in plain language and approved.

### 2. Semantic fidelity and explicit supersession

The controller does not reduce a long discussion to an unmarked paraphrase. It maintains a source-linked semantic kernel containing:

- the current task and north-star outcome;
- approved constraints and acceptance criteria;
- semantic versions and supersession lineage;
- evidence invalidated by a later decision.

A same-outcome revision creates a new semantic version. A changed north star creates a linked successor task.

### 3. Low-fragmentation asynchronous coordination

```text
Stakeholder discusses and approves the outcome
  ↓
Controller sends one cohesive outcome contract
  ↓
Executor works independently after STARTED
  ↓ wakes only on DONE / DECISION_REQUIRED / TIMEBOX_EXCEEDED / BLOCKED
Controller independently samples and reviews evidence
  ↓
Stakeholder is asked only for product-level decisions
```

This reduces controller shadowing, repeated relay summaries, and executor context loss.

### 4. Strategic progress control

The controller maintains a lightweight ledger outside the product repository: north star, milestone, accepted capability, remaining gap, last product progress, auxiliary effort, open decisions, next checkpoint, and stop condition.

Two consecutive checkpoints with only browser, test-harness, selector, or report-tool progress normally close the route and trigger replanning.

### 5. Protection against layered replacement

An atomic replacement is not complete merely because the new implementation runs. Acceptance also requires evidence that:

- supported consumers use the new entry;
- the superseded entry and write path are unreachable;
- current-change orphans are resolved;
- no second independently writable source of truth remains;
- relevant mature user journeys pass.

When overlap is genuinely required, it must be managed as a staged migration with one authority, rollback, monitoring, and an explicit exit condition.

### 6. Evidence-level acceptance

| Evidence level | Proves | Does not prove |
|---|---|---|
| Implementation | Focused logic, build, or unit checks | Supported entry points and user workflow |
| Product path | Real supported input through formal entries | Subjective experience or physical truth |
| User acceptance | Visual, operational, or physical result | Unobserved internal invariants |

“All tests passed,” “the app launches,” and “the screenshot contains output” do not automatically equal user acceptance.

### 7. Route, cost, and rework reporting

When routes materially differ, the controller reports:

- a recommendation and why;
- credible alternatives;
- short-term cost and long-term benefit;
- state debt, rework probability, and rollback difficulty;
- whether stakeholder approval is required;
- the stakeholder's shortest useful reply.

### 8. Verifiable takeover and handoff

A fresh controller does not treat a handoff as permanent truth. It checks a small number of decision-critical facts before dispatch. Handoff state preserves task and semantic lineage, workspace status, executor ownership, evidence level, invalidated conclusions, open decisions, and the next stop condition.

## Good fits

- Software has accumulated multiple state owners, stale entries, and repeated agent rework.
- A stakeholder wants to manage one or more coding agents in plain language.
- A major refactor, replacement, compatibility migration, or cross-session takeover needs governance.
- Executors produce extensive reports but the stakeholder still cannot tell whether the product is usable.
- Long-term goals, active tasks, implementation evidence, and stakeholder decisions must remain distinct.

## Poor fits and overuse risks

- A low-risk, single-step edit that needs no delegation.
- Ordinary Q&A with one executor and no takeover or acceptance complexity.
- Using a skill as a substitute for OS permissions, code review, or real-device safety controls.
- Asking a model to decide product semantics without stakeholder confirmation.

Small tasks may compress the contract and report, but “small” is not a reason to ignore a real product risk.

## Installation

Clone the repository into your personal Codex skills directory.

Windows PowerShell:

```powershell
git clone https://github.com/gaoc77436-bit/product-controller-skill.git "$env:USERPROFILE\.codex\skills\product-controller"
```

macOS or Linux:

```bash
git clone https://github.com/gaoc77436-bit/product-controller-skill.git ~/.codex/skills/product-controller
```

To pin the current supervised beta:

```bash
git checkout v0.1.0-beta.1
```

You can also download the ZIP and place it so this file exists:

```text
~/.codex/skills/product-controller/SKILL.md
```

## Quick start

Explicitly invoke it in a new Codex task:

```text
Use $product-controller.

Act as the controller for this software task. Do not edit product code directly.
First confirm my actual outcome, current evidence, and any decision that truly
belongs to me. Once the outcome is approved, dispatch one cohesive task to an
executor and independently review the result.
```

The skill may also be discovered from requests such as “act as the software controller,” “take over this failed project,” or “supervise the executor task.” Explicit invocation is recommended for high-risk work.

## Updating

For a Git clone installation:

```bash
git pull
```

Production or high-risk work should pin a Release tag and review release notes before upgrading.

## Important limitations

These limits are part of the product, not fine print:

1. **Not a permission sandbox.** It cannot technically prevent a model from writing product code or ignoring instructions.
2. **No zero-rework guarantee.** It reduces semantic drift, state layering, and false acceptance; it cannot guarantee defect-free work.
3. **Shared model blind spots.** Separate executor and verifier roles may still share reasoning biases when they use similar models.
4. **Limited real-world validation.** The current release is backed mainly by synthetic stress tests and independent behavioral audit; full project lifecycles are still accumulating.
5. **Protocol depends on compliance.** Task IDs, semantic versions, deadline generations, and ledgers are behavioral contracts rather than runtime-enforced transactions.
6. **Handoffs can become stale.** Current code, runtime facts, and latest stakeholder feedback must always outrank handoff material.
7. **Final acceptance remains human.** Visual experience, real operation, and physical-device results cannot be declared accepted solely by a model.
8. **Delegation capability is required for execution.** Without an executor or subtask capability, the controller can perform read-only preparation and must then block.

## Current validation status

`v0.1.0-beta.1` has passed supervised-trial checks covering:

- semantic versioning and supersession;
- asynchronous start, pause, continuation, and deadline events;
- stale results from retired executors;
- strategic drift and auxiliary-work stop rules;
- route cost and rework reporting;
- atomic replacement, multi-state, and difficult acceptance paths;
- dirty-worktree protection and controller handoff;
- independent release audit.

These results show that the contract improves decisions in evaluated scenarios. They do not prove reliable behavior in every real project.

## Repository layout

```text
product-controller/
|-- SKILL.md
|-- agents/openai.yaml
|-- README.md
|-- README_EN.md
`-- references/
    |-- operating-contract.md
    |-- coordination-and-strategy.md
    |-- replacement-and-acceptance.md
    |-- evaluation-cases.md
    `-- evaluation.md
```

## Feedback and contributions

Open an Issue for misrouting, conflicting rules, unnecessary process weight, semantic drift, or real project failures. Useful reports include the original goal, observed behavior, expected behavior, risk, and a minimal reproduction.

## License

[MIT](LICENSE)
