---
name: codebase-design
description: Carries this repo's vocabulary and preferences for module design — deep modules, seams, testing through the interface. Use when designing or improving a module's interface, looking for deepening opportunities, deciding where a seam goes, making code more testable or easier for an agent to navigate, or when another skill needs the deep-module vocabulary.
metadata:
  upstream: mattpocock/skills skills/engineering/codebase-design
  upstream-commit: "321658273cb1d20b76026717d027d505790106d4"
  adopted: "2026-09-22"
---

# Codebase design

Two terms other skills here rely on, and the design preferences behind them.

**Deep module**: a module whose interface is small next to the behaviour behind it, so a caller learns little and gets a lot. Interface means everything a caller has to know — signature, invariants, ordering, error modes, required configuration, performance — not just the type surface.

**Seam**: a place where behaviour can be swapped without editing in that place. It is where a module's interface lives, and where it goes is a decision separate from what sits behind it.

## What the user wants from a design

- Depth over breadth in the interface. A useful check when it is unclear whether a module earns its place: imagine deleting it. If the complexity simply disappears, it was a pass-through; if it reappears across the callers, it was carrying something.
- Seams at the boundary where change actually happens. One adapter means a hypothetical seam; two mean a real one. A module may keep internal seams its own tests use without exposing them at its interface.
- Tests that cross the same interface the callers do. Wanting to test past the interface is usually a sign the module is the wrong shape, and tests written against internals have to be rewritten on every refactor.

## Designing it twice

The first interface that comes to mind is rarely the best one. Where the shape matters and the cost of getting it wrong is real, sketching two or three genuinely different interfaces — one minimal, one flexible, one tuned for the commonest caller — and comparing them on depth and seam placement has produced better designs than iterating on the first. Subagents can draft them in parallel. The user wants a recommendation out of that, not a menu.
