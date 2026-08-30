# Replacement and acceptance

Read this reference when a change replaces, supersedes, migrates, or claims completion for an existing responsibility.

## Classify the change

| Type | Meaning | Completion condition |
|---|---|---|
| Fix | Same owner and interface; wrong behavior corrected | Root cause fixed and regression evidence passes |
| Extend | Existing owner gains a new behavior | No new writable truth or competing entry point |
| Atomic replace | A local implementation assumes a responsibility with no external/version overlap | New path proven; superseded path and current-change orphans removed safely |
| Staged migrate | External/versioned consumers or data/deployment safety require overlap | Each stage verified; whole migration complete only after final contract/removal |

Calling a replacement an “extension” does not make coexistence acceptable.

## Current-change orphan rule

A current-change orphan is code, an import, test, adapter, renderer, entry point, state writer, or document used at the task base revision and made unused by the current atomic change. Remove it in the same accepted task.

Establish the task base before product writes:

- Clean worktree: the exact starting revision plus branch/worktree identity.
- Dirty worktree: an immutable pre-task snapshot of staged, unstaged, and relevant untracked files, with ownership recorded; perform agent work in an isolated worktree or equivalent copy.

Do not retroactively call current `HEAD` the task base when pre-task WIP existed. If the baseline or change ownership cannot be reconstructed, mixed dirty files are protected: do not classify their code as current-change orphan, do not authorize deletion, and do not accept the replacement as complete. Recover the pre-task state from backups/history/patches or redo the replacement from a known revision in isolation. Only the stakeholder may accept irreversible WIP loss.

This rule does **not** authorize deletion of unrelated pre-existing dead code. Mention unrelated debt without deleting it.

Before deletion, inventory consumers across relevant code, HTML/templates, configuration, tests, dynamic registries, and string-based entry points. If consumer identity remains uncertain:

1. Do not delete blindly.
2. Do not add or keep a second writable implementation as insurance.
3. Stop and report the exact unknown consumer boundary.

## Atomic replacement completion contract

A replacement is accepted only when all are true:

- The new owner and entry point are named.
- Formal consumers use the new path.
- The superseded write/entry path is no longer reachable.
- Current-change orphans are removed.
- Compatibility adapters, if genuinely required, have named consumers and forward to the single owner without independent persistence or business rules.
- Focused checks and the relevant mature user journey pass.

“Hidden,” `display:none`, no current DOM caller, feature-flagged off, deprecated, or documented for later deletion does not satisfy removal when the current change made the code obsolete.

## Controlled staged migration

Use staged migration only when an observable constraint prevents atomic replacement: published API consumers, multiple deployed versions, rolling release, durable-data migration, or a required rollback window.

Every stage records:

- one authoritative business source for that stage;
- allowed adapters/projections/copies and their named consumers;
- how divergent writes are prevented or reconciled;
- entry evidence, exit evidence, monitoring, and rollback;
- the final contract step that removes deprecated routes, fields, and migration-only code.

Typical data stages are expand → backfill → cutover → contract. A v1 adapter may accept writes only by forwarding them to the same canonical command/owner used by v2. A projection, cache, replica, or backfill target is not a second truth when it is derived, non-authoritative, and cannot independently accept business decisions.

Application dual-write to independent authorities is prohibited by default. If a migration genuinely requires mirrored writes, it needs an approved migration design with a named authority, idempotency, reconciliation, failure handling, rollback, monitoring, and a dated exit condition.

Stage reports say **stage verified / migration incomplete**. “Replacement complete” is reserved for the final removal gate after external consumers, old binaries, rollback windows, and old data readers/writers satisfy their exit criteria.

## Evidence levels

| Level | Proves | Does not prove |
|---|---|---|
| Implementation | Focused logic/build checks | Formal integration or user workflow |
| Product path | Real supported input through formal entry points | Subjective UX or physical truth |
| User acceptance | Visual, operational, or physical result | Unobserved internal invariants |

Mocks, direct state injection, object existence, callback invocation, screenshots missing required interaction, alternate test pages, or application startup may support diagnosis. They do not alone prove the product path.

## Auxiliary-work budget

Browser automation, profiles, selectors, fixtures, report exporters, and audit tooling are auxiliary. After one failed attempt, allow at most one correction for the same auxiliary responsibility and acceptance objective. A second failure closes that path; changing tool, script, profile, port, or error wording does not reset the count. A new product observation that changes the root-cause hypothesis is new evidence, not an auxiliary retry.

## Rationalizations and reality

| Rationalization | Reality |
|---|---|
| “The old renderer is not called now; cleanup can wait.” | If this change made it obsolete, leaving it is an incomplete replacement and a future reactivation risk. |
| “Keeping both paths is safer during migration.” | Overlap is allowed only as a controlled stage with one authority, named compatibility behavior, exit evidence, and rollback. |
| “Deleting is risky, so keep it deprecated.” | Inventory consumers. Unknown consumers are a blocker, not permission to retain a competing implementation. |
| “Tests are green, so coexistence is controlled.” | Synchronization tests prove selected cases, not a single owner. |
| “A screenshot proves the screen works.” | It proves only what is visible in that frame, not the required interaction or persistence. |

## Red flags

- Unbounded later cleanup, unnamed dual write, fallback owner, or compatibility store
- A new renderer/controller/state container for an existing responsibility
- Current-change orphan retained because of deadline or sunk cost
- Acceptance evidence produced by writing the expected state directly
- Third attempt on the same auxiliary layer

Any red flag blocks acceptance until it is resolved or elevated as a genuine product-level decision.
