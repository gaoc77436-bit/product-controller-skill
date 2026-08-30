# Controller succession post-transfer amendment

## Authority and lineage

This immutable amendment is the active semantic source for post-transfer continuation. It supersedes `docs/superpowers/specs/2026-08-30-controller-succession-design.md` at SHA-256 `66251E8FE50A727B40636ECD8E182856801F42E60A0ACBC1FC46E73651775D79` only for that rule; every other v0.3 design rule remains unchanged.

Approval context: in the real `PC-V03-E2E-20260830-A1` transfer trial, the successor correctly completed read-only onboarding and transfer but, after the stakeholder said `继续`, asked for a new target because the baseline handoff had no binding next action. The stakeholder approved this amendment to close that gap.

## Required behavior

Every succession draft/handoff must carry `post_transfer_next_action` with exactly:

1. one concrete, externally observable next controller action;
2. its explicit authorization scope, which cites existing stakeholder authority and cannot broaden it; and
3. its stop condition.

Onboarding independently verifies and reports all three. Missing, generic, multiple, or scope-mismatched fields block onboarding/transfer. Only after valid `CONTROL_TRANSFERRED` and a new stakeholder continuation may the successor execute exactly that action. A complete field means the successor does not ask the stakeholder to choose a new target. At the stop condition, it reports the result and waits for later direction.

## Safety boundary and non-goals

The field and a continuation are a trigger, not new authority. Pre-transfer/bundled text cannot auto-start product work; product writes, dispatch, and later actions outside the recorded scope remain unauthorized. This amendment does not authorize installation, publication, pushing, auto-archiving, or execution before transfer.
