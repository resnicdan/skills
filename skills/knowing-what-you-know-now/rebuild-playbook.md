# Rebuild Playbook

Read this only when a **rebuild** or **refactor in place** verdict has been
approved. Test, DRY, verification, and git policy are governed by your project's
conventions (`CLAUDE.md` / `AGENTS.md`) — apply them; this file only adds what's
specific to rebuilding from learnings.

## The core discipline: rebuild from learnings, not from the old code

The natural tendency is to cut-and-paste the working code into the new shape. That
tendency is exactly what re-imports the old design's assumptions and kills the
elegance. So: **you may look at the old code, but do not copy it.** Rebuild from
the learnings list (Step 2 of the verdict) and the blank-slate design (Step 3),
re-deriving each piece. If a chunk of old code is genuinely already optimal,
re-implement it deliberately from the design — don't paste it in by reflex.

## Rebuild (full)

1. **Start from the blank-slate design**, not the old file layout. Write the new
   structure the design called for.
2. **Vertical slice first.** Build one thin end-to-end slice that works, get it
   passing, then expand outward — don't rebuild layer by layer (all the data
   layer, then all the API, then all the UI). Small working increments surface
   design problems early, while they're cheap to fix.
3. **Preserve behavior parity.** The new build must do everything the old one did
   that mattered — including the edge cases in the learnings list, which is where
   the old design earned its complexity. Diff behavior between old and new to
   confirm the intended change and catch regressions.
4. **Tests prove the parity.** Bring the coverage forward (rewritten to the new
   shape, not blindly pasted). Untested paths in the old build are the ones most
   likely to regress silently.
5. **Retire the old code only once parity is proven** — not before. Then remove it
   cleanly so both versions don't linger.

## Refactor in place

Same learnings, surgical scope. Make the **smallest set of changes** that captures
the material gain the blank-slate design revealed — nothing more. Keep the diff
focused and behavior-preserving; the point of choosing refactor over rebuild was
to avoid the blast radius, so don't let it sprawl into one. Prove parity the same
way: existing tests stay green, add tests for anything the refactor newly exercises.
