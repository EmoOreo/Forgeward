# DATA_SCHEMA.md

## Purpose

Define the minimum logical data shapes for version 0.1. These are conceptual contracts, not final C# class declarations. Implementations may adapt names to Godot/C# conventions, but must preserve the required behavior and invariants.

## 1. Resource definition

Required fields:
- `id`: stable unique string.
- `display_name`: player-facing name.
- `description`: short explanatory text.
- `category`: currency, material, component, or magical resource.
- `starting_amount` or scenario-provided starting amount, where applicable.
- `minimum_allowed`: normally zero.

Initial IDs:
- `coins`
- `scrap_metal`
- `timber`
- `gears`
- `crystal_shards`
- `labor_capacity`

Labor is a capacity value, not a spendable physical item. Its recovery/refresh rule must be defined by the scenario and day-resolution rules.

## 2. Inventory state

Required fields:
- Resource balances keyed by stable resource ID.
- Any reservations keyed by contract/job ID.
- Optional resource change ledger for shift reports.

Invariants:
- No unknown resource IDs.
- No negative balances unless debt is deliberately introduced and documented.
- Spending is atomic: either the full transaction succeeds or nothing changes.
- Reserved amounts cannot be spent by another action.
- A failed transaction must explain which resource is missing.

## 3. Module definition

Required fields:
- `id`
- `display_name`
- `description`
- `placement_size` or occupied grid cells
- `ports`: port ID, direction, supported type, and connection capacity
- `construction_cost`: resource amounts
- `inputs_per_cycle`
- `outputs_per_cycle`
- `power_requirement` or power output rule
- `labor_cost`
- `capacity`
- `efficiency_rule`
- `wear_per_cycle`
- `durability_max`
- `tags`

Optional fields may be omitted if the mechanic is not used. Do not implement unused fields speculatively.

### Initial module IDs
- `waterwheel`
- `boiler`
- `crystal_generator`
- `gearbox`
- `pulley`
- `saw`
- `mill`
- `pump`

## 4. Module instance

Required fields:
- `instance_id`: stable within the run.
- `module_definition_id`
- `grid_position`
- `rotation`
- `durability` or wear state.
- Any explicitly required runtime state.

The display name is not an identifier. Runtime state must validate against the module definition.

## 5. Connection

Required fields:
- `source_instance_id`
- `source_port_id`
- `destination_instance_id`
- `destination_port_id`

Validation:
- Referenced instances and ports exist.
- Port types and directions are compatible.
- Capacity constraints are respected.
- Unsupported cycles are rejected or handled by a specified algorithm.
- No duplicate connection unless a module/port explicitly allows it.

## 6. Contract template

Required fields:
- `id`
- `display_name`
- `requester_id`
- `description`
- `eligibility_tags`
- `required_outputs` or explicit action requirements
- `quantity`
- `deadline_day` or deadline offset
- `reward`
- `failure_penalty`
- `partial_fulfillment_allowed`
- `optional_conditions`
- `generation_bounds`

Optional condition records should define a condition ID, description, machine/resource rule, and bonus reward. Conditions must be machine-checkable in the MVP.

## 7. Contract instance

Required fields:
- `instance_id`
- `template_id`
- `requester_id`
- `required_outputs`
- `quantity`
- `deadline_day`
- `reward`
- `failure_penalty`
- `status`: available, accepted, fulfilled, partial, failed, rejected, or expired.
- `progress`
- `seed` or generation context sufficient to reproduce the instance.

Contract generation must not silently alter authored requirements outside documented bounds.

## 8. Requester/stakeholder definition

Required fields:
- `id`
- `display_name`
- `description`
- `base_reward_modifier` or equivalent if used
- `reputation_threshold_rules`, if used
- `available_contract_tags`

Initial requester IDs:
- `crown`
- `guilds`
- `villages`

Keep reputation as a simple numeric value per group in version 0.1. Avoid simulating complex political relationships.

## 9. Event template

Required fields:
- `id`
- `display_name`
- `description`
- `eligibility_conditions`
- `effect`
- `weight` or explicit selection rule
- `once_per_run` or repeat policy
- `tags`

Events must apply explicit, validated state changes. Randomness must come from the seeded run generator.

## 10. Scenario/objective

Required fields:
- `id`
- `display_name`
- `description`
- `starting_inventory`
- `starting_modules` or starting blueprint availability
- `contract_pool_tags`
- `event_pool_tags`
- `objective_definition`
- `terminal_conditions`
- `generation_constraints`

Scenario validation must check that the objective has at least one plausible route to completion under its authored constraints. A deliberately high-risk scenario must be explicitly identified as such.

## 11. Run state

Required fields:
- schema version
- run seed
- current day
- terminal status
- scenario/objective ID
- inventory and reservations
- contract instances and statuses
- machine instances and connections
- stakeholder reputation
- objective progress
- event history
- current report/history needed for UI and reproducibility

## 12. Persistent progression state

Required fields:
- schema version
- unlocked blueprint IDs
- unlocked contract categories
- starting options/unlocks
- settings if stored with progression

Run-local currency, contract state, and machine wear must not become permanent unless a specific progression rule explicitly says so.

## 13. Validation and error behavior

- Reject duplicate stable IDs.
- Reject unknown references.
- Reject malformed numeric values and invalid resource amounts.
- Reject unsupported graph cycles in the initial implementation.
- Provide actionable validation messages.
- Never silently convert invalid values into plausible-looking defaults if that could change game outcomes.
- Include a schema version so saves can be migrated or rejected safely.
