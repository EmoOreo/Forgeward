# Decision 0001 — Grid-based modular machines with deterministic simulation

**Status:** Accepted for MVP; revisit only with evidence from playtesting.  
**Date:** 2026-10-14

## Context
The game needs creative machine building, but the first version must remain feasible for a solo developer relying heavily on AI-assisted implementation. Unrestricted physics, realistic fluid flow, rope behavior, and continuous simulation increase debugging and testing costs.

## Decision
Use a 2D workshop grid with typed connection ports and a deterministic, discrete-step simulation. Represent modules as nodes and connections as edges. Use explicit rules for inputs, outputs, power, labor, capacity, efficiency, wear, and failure.

## Consequences
Benefits:
- Easier to test and reproduce.
- Machine arrangements remain meaningfully different.
- Invalid assemblies can be explained to the player.
- The system can be extended after the MVP is proven.

Costs:
- Less physical freedom than a full physics sandbox.
- Some behavior is abstracted rather than physically emergent.
- Module rules must be designed carefully to avoid feeling like arbitrary recipes.

## Revisit when
Playtesting demonstrates that grid/port construction cannot deliver the intended creative engineering fantasy, and there is a concrete, affordable alternative with a defined test plan.
