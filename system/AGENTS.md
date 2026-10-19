# {{OWNER}}-dev

This is {{OWNER}}'s workspace. Claude Code and Codex both read this file:
Codex reads it directly, and Claude Code reads it through `CLAUDE.md`,
which only contains `@AGENTS.md`. When you change how this workspace works,
change it here so that both agents stay in step.

Workspace root: `{{ROOT}}`

## Sessions

Each session starts and ends the same way:

1. **`/resume`** (`$resume` in Codex) at the start. It reads the nearest
   `STATE.md`, runs a few quick checks and suggests what to do next. It
   changes nothing.
2. Do the work.
3. **`/handoff`** (`$handoff` in Codex) at the end. It rewrites `STATE.md`
   so the next session can pick up where this one stopped, queues anything
   worth remembering, and commits.

If you're short on time, a plain commit is enough. Make a note of the
skipped handoff the next time you're in.

## One writer, one reviewer

Two agents can work on the same project, but only one of them changes it.

- The **writer** pane edits files, commits and runs `/handoff`.
- The **reviewer** pane reads, reviews, plans and answers questions. It
  never edits files, commits or runs `/handoff`. Its suggestions go to the
  writer or to {{OWNER}}.

If you can't tell which role you have, ask before editing anything.

## Model routing

Run the main session on the strongest model, and keep that model for the
work only it should do: planning, judgment calls, putting results
together, and decisions that depend on everything else. Hand the rest to
a subagent on the cheapest model that can do it reliably.

| Model | Good for | Subagent |
|---|---|---|
| Haiku | searching, listing, inventories, pulling facts out of files when it's clear what counts as right | `scout` |
| Sonnet | routine coding, drafting, editing and moving files | `builder` |
| Opus | analysis that needs judgment, reading messy or legal material | `analyst` |
| the main model | planning and orchestration, or anything where explaining the task would take longer than doing it | none |

- **Small jobs stay in the main session.** A single lookup or a one-line
  edit costs less done directly than briefed to a subagent. Delegate when
  the work is substantial: many files, long output, a long read.
- **If you're not sure which tier, go one up.** A plausible-but-wrong
  answer from a model that was too small costs more to fix than the
  tokens it saved.
- **Subagents return a digest, not a dump.** A subagent with a lot to say
  writes it to a file in `$TMPDIR` and returns the path plus a summary of
  ten lines or fewer. The main session opens the file only if the summary
  doesn't settle it.
- **Don't switch `/model` in the middle of a session.** The prompt cache
  is per model, so switching makes the whole conversation get re-sent at
  full price. For a cheaper model, use a subagent. To change the main
  model, start a new session.
- **Keep sessions short.** A long conversation is re-read on every turn,
  and that adds up faster than model choice does. At a natural break, run
  `/handoff` and then `/clear`, and carry on from `/resume`.

This applies to Claude Code. Codex has no subagent models here, so it
ignores the Subagent column.

## Where things go

Every piece of information has exactly one home:

| What | Where |
|---|---|
| Where we are, what's next | `STATE.md` in that project. Overwrite it each time, never append. |
| A decision and its reasons | `DECISIONS.md` in that project. Append only. |
| A lasting fact about {{OWNER}} or this machine | `memory/`, but only through `/curate`. To add one, put a line in `memory/_inbox.md`. |
| What changed | a git commit |

Don't record the same fact in two places. Copies drift apart.

### STATE.md

```markdown
---
updated: YYYY-MM-DD
status: active | paused | done
next: "the single most useful next step"
---

# STATE — <project>

## Where we are
<a few lines: what was done, where it stopped, anything blocked>

## Next — pick one
1. **<action>** — <rough size>; <why it matters>. ← recommended
2. <action> — <rough size>; <why>
```

### DECISIONS.md

Add an entry as soon as a choice is settled, not at the end of the
session:

```markdown
## D-003 · YYYY-MM-DD · <the choice in a few words>
**Chose:** <what>. **Over:** <the alternatives>. **Because:** <why>.
```

Number entries in order. Never edit or delete an old entry. If a decision
changes, add a new entry that says "replaces D-00n".

## Memory

`memory/` holds lasting facts: preferences, corrections, and how this
machine is set up. Task progress, finished work and to-dos don't belong
there, since they go in STATE or in a commit.

- Anyone can add a line to `memory/_inbox.md`.
- Only `/curate` writes the other files in `memory/`.
- To recall something, read `memory/index.md` first and then open only
  the file you need. Don't read the whole folder.
- A fact you aren't sure of is worse than no fact at all. Check it before
  you save it.

## Git

- Every project is a git repo from the day it starts, and so is this
  workspace.
- Commit often, at the end of each meaningful piece of work. Don't save
  everything up for one big commit.
- Messages follow Conventional Commits, in lowercase:
  `type(scope): summary`, with type one of `feat fix refactor docs test
  chore build ci`.
- Pushing is for backup or sharing. A local commit you haven't pushed is
  not at risk.

## Secrets

Never put a secret in any repo, committed or not: passwords, API keys,
tokens, private keys, `.env` files with real values. Keep them in a
password manager. If one ever gets committed, treat it as leaked: remove
it and rotate it (change the password or issue a new key). Removing it
from the files is not enough on its own.

## Projects

Each project has its own `AGENTS.md`, which says what the project is and
points back here. The rules in this file apply to every project unless the
project's own `AGENTS.md` says otherwise.

<!-- project list: maintained by new-project -->
