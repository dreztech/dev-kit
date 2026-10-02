# dev-kit

A small starter system for working with Claude Code and Codex from the
terminal on a Mac. It includes:

- **Session bookends:** `/resume` to start a session and `/handoff` to end
  it.
- **Per-project files:** a `STATE.md` resume point and a `DECISIONS.md`
  decision log in each project.
- **One instruction file** (`AGENTS.md`) and one skills folder, both
  shared by the two agents.
- **A curated memory folder:** facts go to an inbox, and `/curate` files
  them.

## Install

```sh
git clone https://github.com/dreztech/dev-kit.git
cd dev-kit
claude
```

Then tell Claude: **"run SETUP.md"**.

SETUP asks for your name and your projects, checks your machine, and
builds `~/<name>-dev` as a fresh git repo of its own. It links the skills
into both Claude Code and Codex, and brings your projects in. Once it's
done, this folder isn't needed any more and you can delete it.

Read `GETTING-STARTED.md` in your new workspace for the daily routine.

## What's in here

| Path | What it is |
|---|---|
| `SETUP.md` | the install steps the agent follows |
| `system/` | the workspace template that gets copied to `~/<name>-dev` |
| `system/skills/` | `resume` `handoff` `curate` `clarify` `new-project` |
| `system/templates/project/` | the files every project gets |

## Requirements

macOS, git, and Claude Code. Codex and
[herdr](https://github.com/ogulcancelik/herdr) are optional but
recommended.

## Status

Provided as-is. Once installed, the workspace is yours to change. Nothing
syncs back to this repo, and no maintenance or support is promised.
MIT-licensed.
