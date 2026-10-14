# ROADMAP.md

This roadmap is milestone-based. Estimates should be added only after inspecting the repository and validating the development environment.

## Milestone 0 — Pre-production and environment verification
- Review and approve the design and architecture documents.
- Audit the documents with an independent report-only reviewer.
- Verify Godot 4 C# support and the .NET SDK.
- Initialize a minimal project and verify it launches.
- Establish the test/build workflow.

**Exit:** a clean, reproducible project environment exists and the documents agree.

## Milestone 1 — Economy and contracts
- Resource balances and validated transactions.
- Contract acceptance, rejection, deadlines, rewards, and penalties.
- Fixed-order day resolution.
- Minimal main objective and terminal conditions.
- Automated domain tests.

**Exit:** a test-driven seven-day economy loop works without machine UI.

## Milestone 2 — Machine graph and production
- Grid/module representation.
- Typed ports and connection validation.
- Basic power and production rules.
- Wear and understandable failure reasons.
- Deterministic simulation tests.

**Exit:** at least three distinct machine arrangements produce different, valid outcomes.

## Milestone 3 — Integrated playable slice
- Desk UI and inventory display.
- Workshop UI and connection editing.
- Contract requirements linked to machine output.
- Shift report and win/loss flow.

**Exit:** a complete run is playable from start to finish through the UI.

## Milestone 4 — Roguelike replayability
- Seeded scenarios.
- Validated contract and event templates.
- Final assessment.
- Run summary.
- Persistent blueprint/starting-option unlocks.
- Save/load validation.

**Exit:** different seeds create meaningful, valid variation and reported issues are reproducible.

## Milestone 5 — Usability and first release candidate
- Tutorial prompts and clear validation messages.
- Balance pass and softlock checks.
- Original or properly licensed visual/audio assets.
- Asset provenance and license inventory.
- Desktop export and fresh-install test.
- External playtest.

**Exit:** a new player can finish a run without developer help.

## Possible later expansions (not committed)
- More modules and contract types.
- More stakeholder groups or locations.
- Additional environmental modifiers.
- More spatially expressive construction.
- More advanced physics or fluid/heat systems.
- Optional richer worker simulation.
- Mod support.

An expansion should be added only when the current game has evidence of a specific limitation that the feature would solve.
