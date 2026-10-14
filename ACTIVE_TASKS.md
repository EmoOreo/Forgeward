# ACTIVE_TASKS.md

## Rules for task execution

- Work only on the current task unless explicitly authorized.
- Read `README.md`, `CURRENT_STATE.md`, `ARCHITECTURE.md`, `GAME_DESIGN.md`, and `DATA_SCHEMA.md` before changing code.
- Do not invent new mechanics or expand scope.
- Inspect the repository and build environment before editing.
- Make the smallest coherent change.
- Add or update tests for changed behavior.
- Run the relevant tests and build; report commands and actual results.
- Report changed files, assumptions, risks, and anything not verified.
- Do not claim completion if a build or test was not run.
- Update `CURRENT_STATE.md` only with verified facts.
- Do not modify the design/architecture documents to rationalize an implementation deviation. Propose the change separately for approval.

## Current task

**TASK-000 — Documentation baseline and environment check**

Status: Ready  
Implementation status: No game code is assumed to exist.

### Goal
Confirm the repository and development environment, then prepare a clean Godot 4 + C# project without implementing gameplay systems.

### Steps
1. Inspect the repository state and current files.
2. Confirm installed Godot version, .NET SDK availability, and target platform.
3. Confirm whether the chosen Godot installation supports C#.
4. Initialize the smallest valid Godot C# project only if the environment supports it.
5. Add a minimal boot scene that launches successfully, if this can be verified in the current environment.
6. Preserve all existing user files; do not overwrite unknown content.
7. Record actual versions, commands, results, and blockers in `CURRENT_STATE.md`.

### Acceptance criteria
- Existing repository contents are inspected before changes.
- No existing files are deleted or overwritten without explicit approval.
- Engine and SDK versions are recorded.
- The project opens or launches successfully, or a precise blocker is documented.
- No inventory, contract, machine, progression, or other gameplay systems are implemented in this task.
- A diff/change summary is supplied.

### Out of scope
- Implementing gameplay.
- Adding assets or plugins.
- Choosing a new engine or upgrading the toolchain without approval.
- Claiming a build passed without running it.

## Task queue (not yet authorized)

**TASK-001 — Inventory and economy domain model**
- Implement resource definitions, inventory balances, atomic spending/reward transactions, reservations, and validation.
- Add unit tests for overspending, unknown resources, reservation conflicts, and transaction rollback.
- Acceptance: all economy tests pass and UI is not required to test the domain rules.

**TASK-002 — Contract model and day advancement**
- Implement contract instances, acceptance/rejection, deadlines, completion, failure, and a deterministic day-resolution sequence.
- Add tests for deadline boundaries, accepted contract outcomes, and objective completion.
- Acceptance: a minimal seven-day run can resolve using test data without machine UI.

**TASK-003 — Machine graph model**
- Implement module definitions, instances, grid transforms, typed ports, connections, and validation.
- Add tests for valid/invalid port pairs, capacity, duplicate connections, missing references, and unsupported cycles.
- Acceptance: valid graphs are represented and invalid graphs produce actionable errors.

**TASK-004 — Deterministic production simulation**
- Implement a small, explicit set of production/power rules and wear.
- Add tests for waterwheel → gearbox → mill and crystal generator → pump, including insufficient inputs.
- Acceptance: repeated identical inputs and seed produce identical results.

**TASK-005 — Basic desk UI**
- Display inventory, available contracts, accepted contracts, deadlines, and clear accept/reject actions.
- Acceptance: player actions call domain services; UI cannot bypass validation.

**TASK-006 — Basic workshop UI**
- Add grid placement, rotation, connection editing, and validation feedback.
- Acceptance: at least three different working assemblies can be created and resolved.

**TASK-007 — Shift report and terminal states**
- Display resource changes, production, contract outcomes, wear, reputation changes, and run status.
- Acceptance: complete seven-day run can be won or lost through the UI.

**TASK-008 — Seeded scenarios and content validation**
- Add authored contract/event templates, run seed, scenario constraints, and generation validation.
- Acceptance: several seeds create varied, valid runs and failures are reproducible.

**TASK-009 — Save/load and progression**
- Add versioned run saves and persistent unlocks with validation.
- Acceptance: valid state survives restart; invalid/old data fails safely or migrates according to a documented policy.

**TASK-010 — MVP polish and release check**
- Add tutorial guidance, feedback, asset provenance, balance pass, and desktop export.
- Acceptance: a new tester can complete a run without developer intervention.

## Task reporting template

For every task, report:
1. Summary of change.
2. Files changed.
3. Tests/build commands run.
4. Actual test/build results.
5. Acceptance criteria passed/failed/not verified.
6. Known limitations and follow-up tasks.
7. Commit or pull request reference, if applicable.
