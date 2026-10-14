# ARCHITECTURE.md

## 1. Technical baseline

- Engine: Godot 4.x with C#/.NET support.
- Game type: single-player, 2D, desktop-first.
- Workshop: grid placement with typed connection ports.
- Simulation: deterministic, discrete-step rules; no unrestricted physics requirement.
- Persistence: local save files for run state and meta-progression.
- Content: data-driven records where this simplifies balancing and testing.

Confirm the exact Godot/.NET versions and build environment before implementation. Do not upgrade engine versions mid-task without recording the reason and checking compatibility.

## 2. Architectural principles

1. Game rules must not depend on UI nodes.
2. Simulation must be deterministic for a given state, seed, and player action sequence.
3. Use explicit data types and validation at system boundaries.
4. Resolve a day in a fixed, documented order.
5. Separate run-local state from persistent progression.
6. Keep modules small enough for focused testing and review.
7. Do not add an abstraction until a real requirement justifies it.
8. UI communicates state and requests actions; domain systems validate and apply them.

## 3. Proposed systems

### RunManager
Owns run seed, current day, main objective, run status, and transitions between days and runs.

### InventoryService
Owns resource balances, spending, income, reservations, and validation. Prevents negative balances unless a specifically documented mechanic allows debt.

### ContractManager
Loads contract templates, generates eligible contracts, accepts/rejects requests, checks feasibility constraints, tracks deadlines, and resolves outcomes.

### MachineGraph
Owns module instances, placement, rotation, ports, connections, graph validation, and machine layout serialization.

### SimulationEngine
Evaluates connected machine graphs using explicit rules: available inputs, power/production flow, capacity, efficiency, wear, and failure checks. It must not read UI controls directly.

### DayResolver
Runs the end-of-day transaction in a fixed order:
1. Validate accepted work and machine layout.
2. Reserve or consume inputs for scheduled operations.
3. Evaluate machine graph and production.
4. Apply labor and operating costs.
5. Evaluate contract completion and deadlines.
6. Apply rewards, penalties, and reputation changes.
7. Apply eligible events and wear/failure effects according to documented timing.
8. Check run objective and defeat conditions.
9. Produce a report and advance the day if the run continues.

If a mechanic needs a different order, document it and add tests. Do not let separate systems independently advance the day.

### EventManager
Selects eligible events using the run's seeded random generator and applies explicit, validated effects.

### ProgressionManager
Owns persistent unlocks and starting options. Must not accidentally serialize run-only resources as permanent progression.

### SaveService
Serializes versioned run and progression data, validates data when loading, and handles missing or invalid save files safely.

### UI scenes
Display state and send player actions to the domain systems. They must not directly modify inventory balances or decide contract outcomes.

## 4. Machine graph model

Represent each placed module as a node with:
- Stable instance ID.
- Module definition ID.
- Grid position and rotation.
- Durability/wear state.
- Input/output ports and their types.
- Runtime values needed for the current simulation.

Represent each connection as an edge linking a source port to a destination port. Validate:
- Both endpoints exist.
- Port types are compatible.
- Port direction is valid.
- The same port is not over-connected beyond its capacity.
- The connection does not violate module-specific constraints.
- Cycles are either rejected in version 0.1 or handled by a clearly specified bounded algorithm. Do not allow accidental infinite evaluation.

The MVP may reject unsupported cycles with a readable message. A graph should produce the same result regardless of the order in which its nodes were added to the UI.

## 5. Simulation model

Prefer discrete production rules over continuous simulation. A machine definition can specify:
- Input requirements per work cycle.
- Output quantities per work cycle.
- Power/operating requirement.
- Labor cost.
- Capacity.
- Efficiency modifier.
- Wear per cycle.
- Failure threshold or explicitly seeded failure rule.

Power and production must not be created from nothing. Each output should trace to an input, source, or documented conversion rule. For example, a crystal generator consumes crystal resource/charge to produce power. A waterwheel requires available water flow.

Gearboxes and pulleys can use simple, documented modifiers rather than attempting full physical torque simulation in the MVP.

## 6. Determinism

- Store the run seed.
- Use an explicit seeded random generator for gameplay variation.
- Do not use frame timing, animation completion, UI iteration order, or unseeded randomness to determine gameplay outcomes.
- Sort or otherwise stabilize any collection whose iteration order could affect results.
- Keep simulation calculations in pure or mostly pure functions where practical.
- Record enough run state to reproduce a reported issue.

## 7. Data and serialization

Keep definitions separate from runtime state:
- Definitions: module types, resources, contract templates, event templates, objective templates.
- Run state: current inventory, accepted contracts, placed modules, connections, wear, day, seed, reputation, objective progress.
- Persistent state: unlocked blueprints, starting options, settings, schema version.

Use stable string IDs for content records. Never rely on display names as identifiers.

Loading must validate schema versions, required fields, references, and resource bounds. Unknown or invalid content should fail safely with a useful error rather than corrupting a save.

## 8. Suggested repository structure

```text
/
  README.md
  GAME_DESIGN.md
  ARCHITECTURE.md
  DATA_SCHEMA.md
  ACTIVE_TASKS.md
  CURRENT_STATE.md
  ROADMAP.md
  project.godot
  Forgeward.csproj
  src/
    domain/
    systems/
    data/
    ui/
  scenes/
  data/
    resources/
    modules/
    contracts/
    events/
    objectives/
  tests/
  assets/
    original/
    licensed/
  docs/
    decisions/
    audits/
```

Adjust folders to the actual Godot project layout when initialized. Do not create empty abstractions just to fill the tree.

## 9. Testing strategy

### Unit tests
- Inventory cannot overspend.
- Contract deadline and partial-fulfillment rules are correct.
- Port compatibility validation works.
- Production inputs and outputs are correct.
- Day resolution follows its defined order.
- Seeded event generation is reproducible.
- Victory and defeat conditions are correctly evaluated.

### Integration tests
- Waterwheel → gearbox → mill produces flour when inputs and water are available.
- Crystal generator → pump produces the documented effect when supplied.
- An invalid connection prevents the affected assembly from operating and reports why.
- A contract is fulfilled only when its real conditions are met.
- A complete seven-day run reaches a terminal state.

### Regression process
For each task, run the narrow tests first, then the relevant full test/build command. Record the actual commands and outcomes in `CURRENT_STATE.md`. Never claim tests passed if they were not run.

## 10. Failure handling and logging

Show player-facing explanations for invalid builds, insufficient resources, missing inputs, failed production, and missed contracts. Technical errors should also be logged with relevant IDs and run seed, without overwhelming the player.

## 11. Decisions requiring evidence later

Do not pre-commit to:
- A larger physical building model.
- Complex heat, fluid, or pressure simulation.
- More than one workshop location.
- A richer worker simulation.
- A large faction/reputation system.

Use the MVP and playtesting to decide whether these are valuable.
