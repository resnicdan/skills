---
name: knowing-what-you-know-now
description: Given something already built and working, decide whether it's worth rebuilding the elegant way — knowing everything you now know. User-invoked only; run it when a build works as intended and you want an educated call (keep / refactor / rebuild) before moving on. Does the leg work to earn the rebuild, not a reflexive rewrite.
disable-model-invocation: true
argument-hint: "[optional: a PR, directory, module, or diff to evaluate]"
---

# Knowing What You Know Now

Run this on something that is **built and working as intended**. Evaluate whatever
target you're pointed at (a PR, a directory, a module, a diff); if none is given,
infer it from the current conversation, and if it's still unclear, ask which build
to evaluate — don't evaluate air.

The job is a judgment call, not a rewrite: **earn the rebuild**. Most of the time
the honest answer is "keep it" or "one targeted refactor" — a full rebuild has to
justify itself. The guiding move is the whiteboard question: *knowing everything
you know now, how would you build it?* The reason an elegant version can exist is
that **the learning is the collateral, not the code** — you now know things about
the problem you didn't when you started. Surfacing that learning, and testing
whether it changes the shape of the solution, is the whole point.

Engineering policy — DRY, test expectations, the effort/risk/maintenance framing,
verification discipline, git hygiene — is governed by your project's conventions
(e.g. your `CLAUDE.md` / `AGENTS.md`) and your agent's standing preferences. Apply
them; don't restate them here.

## Steps

Do them in order. The order matters: the blank-slate design happens **before** you
re-litigate the current code, so you design from the problem, not from what's
already on the page.

1. **Scope the target.** Pin down exactly what "this build" is and read it — the
   actual code, not a summary. State in one line what it does and what "works as
   intended" means for it (a shipped feature? a working prototype? a spike?). A
   working prototype is not production-grade — note which you're looking at,
   because it changes the bar.

2. **Reconstruct the learnings.** This is the leg work — do it thoroughly, it's
   where the value is. Answer: *what do we know now that we didn't when this
   started?* Constraints discovered mid-build, dead ends, edge cases that forced
   detours, the real shape of the problem vs. the assumed one, places where the
   design bent around something. List them concretely. **The learning is the
   collateral** — this list is the raw material for everything below.

3. **Blank-slate design.** Knowing everything you know now, design the solution
   from a blank slate. Be concrete: name the modules, the data flow, the
   green-path shape among the alternatives you'd choose. **Do not reference or
   reproduce the current structure** — design as if writing it fresh, the way an
   engineer at a whiteboard describes a completely different, simpler
   architecture. Anchoring on the existing shape is the failure mode; resist it.

4. **Compare.** Now put the current build next to the blank-slate design. Where do
   they diverge, and is the divergence **material** (fewer moving parts, less
   coupling, better edge-case handling, simpler mental model) or **cosmetic**?
   Hold both against the bar: *as complex as necessary, as simple as possible.*
   Be specific about the gap — "it could be cleaner" is not a finding.

5. **Verdict — earn the rebuild.** Deliver an educated call, then **stop for
   approval before touching code.** Give all three options, "do nothing" first,
   each with effort / risk / maintenance burden / blast radius, and recommend one:
   - **Keep as-is** — the current build is already the elegant solution, or the
     gap is cosmetic / doesn't justify the cost. Recommend this by default unless
     a rebuild or refactor genuinely beats it.
   - **Refactor in place** — the blank-slate design's gains can be captured by a
     targeted, surgical change without a full rebuild.
   - **Rebuild from learnings** — the blank-slate design is *materially* simpler or
     more robust **and** the gain justifies the cost. This is the option that must
     be earned; don't reach for it reflexively.

## If a rebuild or refactor is approved

Only after the rebuild/refactor is approved, read `rebuild-playbook.md` and follow
it — it holds the discipline for rebuilding from learnings without copy-pasting the
old code. Until then, this skill ends at the verdict.
