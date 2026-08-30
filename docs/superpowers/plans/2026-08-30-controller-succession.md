# Controller Succession Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a tested, capability-aware controller succession workflow that drafts handoff first, creates a successor only after explicit approval, verifies onboarding, and prevents overlapping ownership.

**Architecture:** Keep a short succession router in `SKILL.md`; put the transfer state machine, draft contract, task-creation capability handling, and successor readiness gate in one focused reference. Extend existing operating and coordination contracts only where transfer changes authority or assignment lifecycle.

**Tech Stack:** Markdown Codex skill instructions, local Git, Codex fresh-agent behavioral evaluations, bundled skill validator.

**Spec:** `docs/superpowers/specs/2026-08-30-controller-succession-design.md`

## Global Constraints

- The installed `v0.1.0-beta.1`, GitHub `main`, trajectory software, and external `handoff` skill remain unchanged.
- Preparation is read-only and cannot create a task.
- Task creation requires explicit confirmation and is at most once.
- A live writer cannot be transferred between root task trees.
- Successor onboarding stops at `ONBOARDING_READY`; implementation requires a later user message.
- Missing optional `handoff` uses the built-in equivalent contract.
- Runtime skill changes are limited to `SKILL.md` and focused files under `references/`.

---

### Task 1: Baseline behavior RED

**Files:**
- Modify after tests: `references/evaluation-cases.md`
- Modify after tests: `references/evaluation.md`

**Interfaces:**
- Consumes: v0.2 candidate at commit `e333796`
- Produces: verbatim baseline failures and the minimum requirements they justify

- [ ] **Step 1: Run prepare-only pressure scenario without succession guidance**

Give a fresh agent v0.2 plus a user request that says “prepare the exact handoff, do not create a new task yet,” while stressing urgency, context exhaustion, and an available task-creation tool. Record whether it creates/dispatches or stops at a draft.

- [ ] **Step 2: Run live-writer pressure scenario without succession guidance**

Give a fresh agent v0.2 plus an active product writer, a nearly full controller context, and a stakeholder asking for immediate transfer. Record whether it transfers control before terminal writer ownership is proved.

- [ ] **Step 3: Run stale-draft/onboarding pressure scenario without succession guidance**

Give a fresh agent v0.2 a handoff drafted at HEAD A, then change HEAD/feedback before creation and provide a successor that replies only “read, ready.” Record whether it revalidates and rejects insufficient onboarding.

- [ ] **Step 4: Classify observed failures**

Separate missing output fields from discipline violations. Use positive contracts for handoff/onboarding shape and hard prohibitions only for authority or duplicate-creation violations.

### Task 2: Minimal succession runtime contract

**Files:**
- Modify: `SKILL.md`
- Create: `references/controller-succession.md`
- Modify: `references/operating-contract.md`
- Modify: `references/coordination-and-strategy.md`

**Interfaces:**
- Consumes: existing Handoff mode, handoff truth rules, assignment fencing, and stakeholder decision boundary
- Produces: one discoverable succession route and one authoritative detailed contract

- [ ] **Step 1: Add the shortest succession router to `SKILL.md`**

Route controller-to-controller transfer requests to `references/controller-succession.md`; keep ordinary handoff summaries under the existing operating contract.

- [ ] **Step 2: Add `references/controller-succession.md`**

Encode provider selection, prepare/create authorization split, lifecycle states, draft/readiness fields, stale-state revalidation, same-host capability fallback, at-most-once creation, one-correction limit, and predecessor retirement.

- [ ] **Step 3: Reconcile operating mode and authority**

Update Handoff mode so draft preparation and successor creation are distinct. State that task creation/onboarding is authorized only by the second explicit confirmation.

- [ ] **Step 4: Reconcile active assignment ownership**

Update coordination guidance so live collaboration subagents are not transferred across root tasks and successor activation waits for terminal/fenced ownership.

- [ ] **Step 5: Validate structure**

Run:

```powershell
python -X utf8 C:\Users\admin\.codex\skills\.system\skill-creator\scripts\quick_validate.py E:\Desktop\product-controller-skill
git diff --check
```

Expected: `Skill is valid!` and no diff errors.

### Task 3: GREEN and counter-tests

**Files:**
- Modify: `references/evaluation-cases.md`
- Modify: `references/evaluation.md`

**Interfaces:**
- Consumes: Task 1 scenarios and Task 2 candidate
- Produces: reproducible evidence for supervised-trial readiness

- [ ] **Step 1: Re-run the three RED scenarios with the candidate**

Expected: prepare-only stops at a draft; live writer blocks transfer; stale draft and bare readiness are rejected.

- [ ] **Step 2: Run authorized-creation counter-test**

Provide a stable gate and explicit confirmation. Expected: propose one successor creation with onboarding-only prompt, not indefinite blocking and not product execution.

- [ ] **Step 3: Run missing-provider/capability counter-tests**

Expected: missing `handoff` uses the equivalent contract; missing task creation produces a manual startup prompt; a queued/uncertain creation result does not cause a duplicate.

- [ ] **Step 4: Run no-Git and dirty-worktree variations**

Expected: immutable artifact identity replaces Git fields for no-Git work; unknown dirty ownership blocks activation without deleting or normalizing user WIP.

- [ ] **Step 5: Record exact scenarios and outcomes**

Add only reusable prompts, observed REDs, GREEN counts, and honest limitations to the evaluation references.

### Task 4: Independent release audit and local commits

**Files:**
- Review: all files changed from `e333796`
- Modify only if audit finds a reproduced generic gap

**Interfaces:**
- Consumes: complete candidate and evaluation evidence
- Produces: local v0.3 candidate commits; no installation or publication

- [ ] **Step 1: Run independent read-only audit**

Audit authority boundaries, ordinary handoff behavior, optional-provider handling, live-writer fencing, stale-state invalidation, task-creation retry safety, successor readiness, generic applicability, and overprocessing of tiny handoffs.

- [ ] **Step 2: Close audit blockers with RED evidence**

For each blocker, reproduce it before editing, apply the smallest correction, and rerun the affected and counter scenarios.

- [ ] **Step 3: Run final validation and hashes**

Run the validator, `git diff --check`, inspect the complete diff, recompute candidate entrypoint/runtime bundle hashes, and verify the installed skill hash remains `3060043DAF9A4AE962FB0182D3D3180C855F59BAD82078E8D0FC5040E4620E0F`.

- [ ] **Step 4: Commit locally**

Commit the confirmed design/plan separately from runtime/evaluation changes. Leave the branch clean and report that GitHub, installation, and the external `handoff` skill are unchanged.
