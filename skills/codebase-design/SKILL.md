---
name: codebase-design
description: Shapes software architecture and module boundaries — deep modules, seams, and interface-level tests — when planning a new project or subsystem, doing a major refactor or rewrite, or rethinking an approach that feels overcomplicated or hard to test. Not for UI or visual design.
metadata:
  upstream: mattpocock/skills skills/engineering/codebase-design
  upstream-commit: "5863806dd4e15c63174bee4e7515a4256792ecfa"
  adopted: "2026-09-22"
---

# Codebase design

Preferences for shaping code architecture and module boundaries when building something new, refactoring, or rethinking an approach. The user's instructions take precedence over this skill.

**Deep module**: a module whose interface is small next to the behaviour behind it, so a caller learns little and gets a lot. Interface means everything a caller has to know — signature, invariants, ordering, error modes, required configuration, performance — not just the type surface.

**Seam**: a place where behaviour can be swapped without editing in that place. It is where a module's interface lives, and where it goes is a decision separate from what sits behind it.

## What the user wants from a design

- **Simple v1, depth over breadth.** A useful check when it is unclear whether a module earns its place: imagine deleting it. If the complexity simply disappears, it was a pass-through; if it reappears across the callers, it was carrying something.
- **Seams only where change actually happens.** One adapter means a hypothetical seam; two mean a real one. A module may keep internal seams its own tests use without exposing them at its interface.
- **Tests that cross the same interface the callers do.** Wanting to test past the interface is usually a sign the module is the wrong shape, and tests written against internals have to be rewritten on every refactor.
- **Willingness to replace rather than patch.** Rewrites are cheap when agents write the code; if a cleaner boundary or fresh approach beats layering another workaround onto an awkward design, propose it openly.
- **Revise in place — no rollback shims.** The user ships forward and never rolls back (2026-11-28): update schemas, wire formats and interfaces in place, and delete obsolete decoders, compatibility shims and version-pinned tests with the code they belonged to. Irreplaceable stored data is the exception and still gets a backup or a tested migration.

## Designing it twice

The first architecture or interface that comes to mind is rarely the best one. Where the shape matters and the cost of getting it wrong is real, sketch two or three genuinely different designs — one minimal, one flexible, one tuned for the commonest caller — and compare them on depth and seam placement. Subagents can draft alternatives in parallel, and `council` can challenge the leading choice. Give the user a clear recommendation, not a menu.
