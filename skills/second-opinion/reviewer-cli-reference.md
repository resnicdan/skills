# Reviewer CLI Reference

Read this at Steps 2–3 of `SKILL.md`, when you are about to dispatch the review.
It lists how to invoke each review-capable agent non-interactively. **These CLIs
change** — if a flag is rejected, check the tool's `--help`. Treat this as a
starting point, not gospel.

## The shape they share

Every one of these takes a prompt non-interactively, lets you pick a model, and
returns text you can parse:

**prompt in (`-p` / `exec`) → `--model` to choose → text (or JSON) out.**

So the dispatch is the same everywhere; only the binary and the flag names change.
That is what makes a tool-agnostic skill possible: pick whichever the user has.

## Per-tool invocation

| Tool | Non-interactive invoke | Pick the model | Notes |
|---|---|---|---|
| **Codex** (OpenAI) | `codex exec "<prompt>"`, or `codex exec review --base <branch>` for its tuned review mode | `-m <model>` | `--sandbox read-only` keeps it from editing. `review --base` rejects a prompt arg — use one form or the other, not both. |
| **Gemini** (Google) | `gemini -p "<prompt>"` | `-m <model>` | Runs headless automatically when stdout is not a TTY. |
| **Grok** (xAI) | `grok -p "<prompt>"` | model via flag / config | `--output-format json` for a parseable event stream; `GROK_API_KEY` in env. |
| **Cursor** | `cursor-agent -p "<prompt>"` | `--model <name>` (`--list-models`) | `--output-format text\|json`. |
| **Claude** | `claude -p "<prompt>"` | `--model <name>` | Read-only unless you pass write permissions. |

## Setting a default reviewer

The skill defaults to a different model than the one you're in, falling back to a
fresh run of your own tool. To pin a specific reviewer instead of choosing it each
run, add one line to your agent's conventions file — the file it already loads
every session, so nothing extra needs wiring up:

- **Claude Code** — `CLAUDE.md` (project) or `~/.claude/CLAUDE.md` (global)
- **Codex / other Agent-Skills harnesses** — `AGENTS.md`
- **Cursor** — `.cursor/rules`

Example line:

> When running second-opinion, review with Codex: `codex exec -m <model>`.

The skill reads that as your standing default. A per-run request ("use Gemini this
time") still wins for that run. If your harness has no conventions file, just name
the reviewer when you run the skill.

## Feeding it the diff

Two ways — pick per tool:

- **Let the reviewer read the repo.** `cd` into the target repo first; tools with
  a built-in review mode (e.g. `codex exec review --base <base>`) compute the diff
  themselves.
- **Pipe the diff in.** `git diff <base>...HEAD` and include it in the prompt. More
  portable across tools that have no review mode of their own.

## Isolating the output

Run the reviewer where its raw transcript won't consume your working context: a
subagent if your harness has them, otherwise a separate process whose stdout you
capture to a temp file and distill down to the compact findings list. Strip the
banner / token-footer noise some CLIs print around the actual review.
