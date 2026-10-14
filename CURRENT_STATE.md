# CURRENT_STATE.md

## Verified status

**Project phase:** Pre-production documentation baseline  
**Game implementation:** Not started / not verified  
**Engine installation:** Not verified in this document  
**.NET SDK:** Not verified in this document  
**Build status:** Not run  
**Automated tests:** Not created or run  
**Playable build:** Does not yet exist as a verified artifact

This file deliberately does not claim that the engine, project, build, or any gameplay system is already working.

## Current design decisions

- Working title: FORGEWARD.
- Genre: single-player resource-management roguelike with modular machine building.
- Setting: fantasy industrial kingdom.
- First target: 2D, desktop-first, Godot 4 + C#.
- Run target: seven workdays.
- Initial resources: coins, scrap metal, timber, gears, crystal shards, and abstract labor capacity.
- Initial module target: eight types, with permission to use fewer in the first vertical slice.
- Workshop model: grid placement and typed ports; deterministic graph-based simulation.
- Procedural content: authored templates with bounded seeded variation and feasibility validation.
- First goal: prove that desk decisions and machine construction meaningfully affect one another.

## Current scope exclusions

No open world, unrestricted 3D physics, realistic fluid/heat simulation, individual worker pathfinding, multiplayer, runtime LLM content generation, large faction simulation, or large content catalog in version 0.1.

## Immediate next task

Run `TASK-000` from `ACTIVE_TASKS.md`: inspect the repository and verify the Godot C# and .NET environment before initializing or modifying project files.

## Blockers and unknowns

- Actual repository location and existing contents have not been inspected.
- Exact Godot and .NET versions are unknown.
- Target export platform and build pipeline have not been verified.
- Machine costs, contract rewards, and balance values are design placeholders pending test scenarios.
- The exact C# testing framework should be selected after the environment check.

## Update policy

Only record implementation details after they are verified from the repository or actual test/build output. Include date, evidence, commands, and unresolved blockers when this file is updated.
