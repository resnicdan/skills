# Contributing

Thanks for wanting to add or sharpen a skill. Keep it small and predictable.

## Layout

One folder per skill under `skills/`:

```
skills/<skill-name>/
├── SKILL.md              # the procedure (keep this small)
└── <reference>.md        # optional sibling reference files, one per branch
```

- `SKILL.md` frontmatter: `name`, `description`, and — for skills you run
  deliberately — `disable-model-invocation: true`. Add `argument-hint` if the skill
  takes a target. Avoid tool-specific keys so the skill stays portable.
- Reference files live flat next to `SKILL.md`, linked from it with a one-line
  pointer ("read `<file>.md`").

## The bar

- **Tool-agnostic.** Write "the agent" / "you", not a product name. Don't assume a
  specific harness's paths or config; refer to "your project's conventions
  (`CLAUDE.md` / `AGENTS.md`)" instead of hardcoding one.
- **Small `SKILL.md`.** Steps in the main file; move branching reference material
  (only used on some paths) to a sibling file behind a pointer.
- **No no-ops.** If deleting a line wouldn't change what the agent does, delete it.
- **No sediment.** Don't pile on caveats over time — keep a single source of truth
  for each piece of reference material.
- **Leading words.** Prefer a short, meaning-dense phrase the agent can repeat back
  over a paragraph of instruction.

## Submitting

Open a PR with the new/changed skill and a one-line entry in the README's Skills
table. If a skill is adapted from someone else's idea, credit the source.
