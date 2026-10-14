# FORGEWARD

**Working title:** FORGEWARD  
**Project type:** Single-player fantasy-industrial resource-management roguelike with modular machine building  
**Prototype target:** Desktop, 2D, Godot 4 + C#  
**Status:** Pre-production; no game implementation is assumed to exist.

## Project goal

Build a small, replayable game in which the player manages a struggling industrial depot in a fantasy kingdom. The player reviews orders, allocates scarce resources, constructs or repairs modular machines, resolves each workday, and attempts to survive a short run.

The design combines:
- Consequential daily decisions and document-driven requests inspired by *Papers, Please*.
- Creative modular engineering inspired by the broad appeal of *The Legend of Zelda: Tears of the Kingdom*.
- Short runs, variable scenarios, and persistent unlocks associated with roguelite games.

These are design inspirations, not instructions to copy any game's art, writing, assets, or implementation.

## Current status

This repository is a documentation baseline only. The game, scenes, scripts, assets, and tests have not yet been implemented. Do not describe any feature as implemented until it exists and has been verified.

## Source-of-truth order

When project documents disagree, use this order:

1. `CURRENT_STATE.md` — verified implementation status and current blockers.
2. `ARCHITECTURE.md` — technical boundaries and system responsibilities.
3. `GAME_DESIGN.md` — player experience, rules, and scope.
4. `DATA_SCHEMA.md` — data shapes and validation rules.
5. `ACTIVE_TASKS.md` — current bounded implementation task and acceptance criteria.
6. `ROADMAP.md` — intended future milestones.
7. This `README.md` — orientation only.

If two authoritative documents conflict, do not guess. Report the conflict and request a decision before implementing affected behavior.

## First playable target

A complete seven-day run with:
- A desk screen for reviewing and accepting contracts.
- An inventory and six resource categories.
- A 2D grid-based workshop with compatible connection ports.
- At least three working machine arrangements.
- Deterministic production and contract resolution.
- A run-ending victory or defeat and a summary.
- A small set of validated contract templates and events.

## Development principles

- Keep the first build small, desktop-first, and single-player.
- Prefer deterministic, testable rules over unrestricted physics.
- Use original, self-created, or appropriately licensed assets. Track asset provenance and license terms.
- Keep game rules separate from UI presentation.
- Do not introduce a feature merely because an AI agent suggests it.
- Do not claim a build or test passed unless it was actually run and the result recorded.
- Keep each coding task narrow enough to review in one pull request or checkpoint.
