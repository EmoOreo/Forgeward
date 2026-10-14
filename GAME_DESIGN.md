# GAME_DESIGN.md

## 1. High concept

FORGEWARD is a single-player fantasy-industrial resource-management roguelike. The player operates a small royal engineering depot during a period of infrastructure failure. Each workday brings contracts, supply constraints, equipment problems, and competing obligations. The player must decide what to accept, what to build, and what to leave unresolved.

The desk-management layer creates pressure and consequences. The workshop layer gives the player creative, systemic ways to solve problems. A short run structure makes the game replayable without requiring an open world or a large content pipeline.

## 2. Design pillars

### 2.1 Scarcity creates meaningful choices
Resources should have competing uses. Spending a gear on a profitable order may prevent a repair later. Avoid artificial busywork and avoid making every resource freely renewable.

### 2.2 Engineering is a means of problem-solving
Contracts describe desired outcomes, not a single prescribed machine. Multiple valid machine arrangements should solve some tasks, with different costs, efficiencies, and risks.

### 2.3 Consequences are legible
Players should be able to understand why a contract succeeded, why production stopped, or why a machine failed. Avoid opaque random punishment.

### 2.4 Runs teach and vary
Different seeds change starting stock, contract sequence, events, and environmental conditions. The rules remain consistent. Persistent unlocks expand options rather than making the game automatically easy.

## 3. Player fantasy and setting

The player is the operator of a remote engineering depot in a fantasy industrial kingdom. The world blends familiar mechanical principles with restrained magical technology: waterwheels, gear trains, pulleys, boilers, pumps, mills, and crystal-powered generators.

Magic should obey explicit game rules. A crystal generator is a power source with a defined input and output, not a narrative exception that bypasses the economy.

The player is not expected to manage an entire kingdom. The depot and its daily obligations are the scope of the first version.

## 4. Core loop

Each workday has four phases:

1. **Review:** Read incoming contracts, deliveries, and events. Check stock, deadlines, and requester consequences.
2. **Allocate:** Accept or reject contracts, reserve resources, and choose how to spend labor and materials.
3. **Engineer:** Place, rotate, connect, repair, or modify machine modules. Validate the assembly before committing resources where practical.
4. **Resolve:** Run production and machine simulation in a fixed order, resolve contracts, apply costs and consequences, show a report, and advance the day.

The day cannot resolve while the player is in an invalid or incomplete modal action. The UI must clearly communicate when an action is final.

## 5. MVP run structure

- One run lasts seven in-game days.
- Each run has one main objective chosen from a small authored set.
- Contracts have deadlines, requirements, rewards, penalties, and optional conditions.
- Day 7 presents a final assessment tied to the run objective.
- Victory occurs when the final objective is completed and the depot remains viable.
- Defeat occurs when the depot becomes insolvent, critical infrastructure reaches an unrecoverable failure state, or a mandatory scenario condition is missed.
- Avoid adding multiple complex failure meters in the MVP. Money, equipment condition, and one scenario objective are sufficient.

### Example main objectives
- Deliver a target quantity of processed goods.
- Keep the depot operational through Day 7.
- Complete a major engineering contract.
- Reach a money target while meeting a minimum public-service obligation.

These objectives should be validated for feasibility against each scenario's starting inventory and available contracts.

## 6. Resources

The MVP uses six resource categories:

| Resource | Role | Typical uses |
|---|---|---|
| Coins | Currency | Purchases, wages, fees |
| Scrap metal | Construction material | Repairs, frames, mechanisms |
| Timber | Construction material | Frames, supports, axles |
| Gears | Precision component | Transmissions, machinery repairs |
| Crystal shards | Magical power resource | Crystal-powered equipment |
| Labor | Capacity pool, not an inventory item | Building, repairs, operating jobs |

Labor is an abstract capacity pool in version 0.1. Do not simulate individual workers, pathfinding, schedules, hunger, or personal inventories.

All costs and rewards are data-driven and must be balance-tested. The numbers in example scenarios are illustrative until entered into validated game data.

## 7. Machine module roster

The initial target is eight module types. The first playable slice may use fewer.

1. **Waterwheel:** Converts available water flow into rotational power.
2. **Boiler:** Consumes fuel to provide heat or pressure. Fuel may be represented by a simple cost/resource in the initial implementation.
3. **Crystal generator:** Consumes crystal charge/shards to produce magical power.
4. **Gearbox:** Transfers rotational power with defined speed/torque trade-offs.
5. **Pulley:** Transfers motion or provides a labor-saving lifting function.
6. **Saw:** Consumes input timber-processing work and power to produce a defined output.
7. **Mill:** Consumes grain, power, and labor to produce flour.
8. **Pump:** Uses power to move water between connected sources and destinations.

The first prototype must not model every real-world physical variable. Each module uses explicit inputs, outputs, costs, capacity, efficiency, durability, and supported port types. Only implement properties that affect actual gameplay.

## 8. Contracts and decisions

A contract data record should define:
- Unique ID and display name.
- Requester group.
- Required output or action.
- Quantity and deadline.
- Reward.
- Failure penalty.
- Required resources or machine capabilities, if applicable.
- Optional condition and bonus reward.
- Whether partial fulfillment is allowed.
- Validation constraints and scenario tags.

Contract outcomes:
- **Fulfilled:** all required conditions met.
- **Partially fulfilled:** only if the contract explicitly allows partial fulfillment.
- **Failed:** deadline missed or accepted contract becomes impossible due to the player's choices.

Contracts should be authored from templates with bounded variation. Never generate arbitrary requirements without checking that the scenario has a plausible path to completion. Some explicitly marked high-risk contracts may be difficult, but must not be accidentally impossible.

## 9. Stakeholders

The MVP has three stakeholder groups:
- **The Crown:** can pay well and expects priority.
- **The Guilds:** provide supplies or specialist components and expect profitable work.
- **The Villages:** need essential services and can provide local materials or goodwill.

Track a simple reputation value per group. Reputation thresholds may influence contract availability, rewards, or supply prices. Do not build a complex political simulation in version 0.1.

## 10. Events and procedural variation

Runs use a recorded seed. Seeded generation can affect:
- Starting inventory and money.
- Available starting blueprints.
- Contract sequence and deadlines.
- Weather/environment modifiers.
- A limited selection of events and breakdowns.
- Final objective.

Use authored event templates with explicit effects and eligibility conditions. Events should alter a known rule, not secretly change core mechanics.

## 11. Persistent progression

After a run, unlocks may add:
- Alternative starting modules.
- Additional blueprint options.
- New contract categories.
- Small, bounded discounts or starting-resource choices.

Progression should expand strategic choices rather than simply stack permanent power until the game becomes trivial. Persistent unlocks are separate from run-local state.

## 12. UI targets

MVP screens:
1. **Desk:** contracts, request details, stakeholder reputation, deliveries, deadlines.
2. **Workshop:** grid, module selection, rotation, connections, module properties, validation messages.
3. **Inventory/status panel:** resource balances, reserved resources, labor capacity, machine condition.
4. **Shift report:** production, resource changes, fulfilled/failed contracts, machine failures, consequences.
5. **Run summary/progression:** objective result, decisions, unlocks, and option to start another run.

A player should be able to identify what to do next without reading external documentation.

## 13. MVP acceptance criteria

The first playable build is acceptable when:
- A complete seven-day run can be played from the UI.
- The player can accept or reject contracts and see the consequences.
- Resource balances cannot be spent below permitted limits.
- At least three distinct machine arrangements produce valid, different outcomes.
- Invalid connections explain the reason.
- Machine and contract results are deterministic for the same state and seed.
- The run ends in a valid victory or defeat state.
- The player receives a readable summary.
- A new seed changes scenario content without generating invalid state.
- No required feature depends on paid assets or a paid service.

## 14. Explicitly out of scope for version 0.1

- Open-world exploration.
- Freeform 3D physics construction.
- Realistic fluid dynamics, thermal simulation, or rope physics.
- Individual worker simulation, pathfinding, and needs.
- Multiplayer or online services.
- Runtime LLM-generated dialogue or contracts.
- Hundreds of parts, factions, or contract types.
- Voice acting or bespoke 3D asset production.
- Mod support.
- A large campaign or multiple locations.

Revisit these only after the MVP is playable and playtesting demonstrates a specific need.
