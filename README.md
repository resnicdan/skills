# skills

A small, curated set of agent skills I use day to day, shared with the community.

They're written to be **tool-agnostic** — the SKILL.md format works with any coding
agent that reads Agent Skills (Claude Code, Codex, and others), and the prose talks
about "the agent," not any one product. Nothing here is tied to a specific tool.

Each skill is a folder under [`skills/`](skills/) containing a `SKILL.md` (the
procedure) plus any reference files it needs.

## Skills

| Skill | What it does |
|---|---|
| [knowing-what-you-know-now](skills/knowing-what-you-know-now) | Run it on something already built and working. It reconstructs what you learned building it, designs the version you'd write *knowing everything you know now*, and returns an educated **keep / refactor / rebuild** verdict with effort/risk/maintenance tradeoffs — earning the rebuild rather than reflexively rewriting. |
| [second-opinion](skills/second-opinion) | Run it before you push. It has a second agent — ideally a *different model* than the one that built the code — review your branch, runs your own independent pass, then reconciles the two into one tagged findings list (**agreed / reviewer-only / mine-only**) and a **ship / review-needed / do-not-ship** verdict. A model reviewing its own work goes easy on it; an independent reviewer doesn't. |

## Install

Skills are plain markdown, so adopting one is just putting its folder where your
agent looks for skills:

- **Claude Code** — copy the folder into `~/.claude/skills/`
- **Codex / other Agent-Skills harnesses** — copy it into `~/.agents/skills/`
- **Any agent** — paste the contents of the skill's `SKILL.md` into your prompt

For example:

```sh
git clone https://github.com/resnicdan/skills.git
cp -r skills/skills/knowing-what-you-know-now ~/.claude/skills/
```

If you use the community [`skills`](https://skills.sh) CLI, you can also run:

```sh
npx skills@latest add resnicdan/skills
```

## How these are written

The house style follows a few principles worth stating up front:

- **User-invoked, for predictability.** Skills here set `disable-model-invocation:
  true` — you run them deliberately rather than hoping the agent picks them at the
  right moment.
- **Small `SKILL.md`.** Steps in the main file; branching reference material pushed
  to a sibling file behind a pointer (progressive disclosure). No no-ops (lines the
  agent would follow anyway), no stale sediment.
- **Leading words.** Short, meaning-dense phrases the agent repeats back to itself
  to steer its behavior.

Method credit: Matt Pocock's *"The Missing Manual: How to Write Great Skills."*
The `knowing-what-you-know-now` skill draws on Farhan Thawar and Tobi Lütke's idea
that *the learning is the collateral, not the code* — knowing everything you know
now, how would you build it? The `second-opinion` skill's two-pass reconcile
pattern is adapted from [`robertoecf/adversarial-review`](https://github.com/robertoecf/adversarial-review),
and its "independence over a smarter model" rationale follows the research on LLM
self-preference bias (a model rating its own output higher than an independent
judge would).

See [CONTRIBUTING.md](CONTRIBUTING.md) if you'd like to add or improve a skill.

## License

[MIT](LICENSE)
