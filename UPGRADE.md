# UPGRADE

**For the agent running this:** this adds what's new since v0.1 to a
workspace that SETUP already built on this Mac. It doesn't touch the
user's projects, `STATE.md`, `DECISIONS.md` or memory. Run commands
yourself and show the user what you find. Ask before changing any file
outside the workspace. If a step fails, stop, explain what went wrong and
how to fix it, and don't skip ahead.

`<kit>` means this repo, the folder you are running in.

## What's new in v0.1.1

Model routing for Claude Code: a `Model routing` section in `AGENTS.md`,
and three subagents on cheaper models (`scout` on Haiku, `builder` on
Sonnet, `analyst` on Opus) that the main session hands work to.

## 1. Find the workspace

The workspace is the folder named in the `@…/AGENTS.md` line of
`~/.claude/CLAUDE.md`:

```sh
grep -n 'AGENTS.md' ~/.claude/CLAUDE.md
```

From here on, `<root>` means that folder's full path (use `$HOME`, not
`~`). Confirm it with the user. Then check it is clean:

```sh
git -C "<root>" status --short
```

If that prints anything, stop and ask the user to commit or run
`/handoff` there first, so this upgrade lands as its own commit.

## 2. Add the subagents

If `<root>/agents/` already exists, stop and ask.

```sh
cp -R "<kit>/system/agents" "<root>/agents"
```

## 3. Add the rules

The user may have edited their `AGENTS.md`, so add the section instead of
replacing the file.

1. If `<root>/AGENTS.md` already has a `## Model routing` heading, show
   it to the user and ask whether to replace it. Otherwise:
2. Copy the whole `## Model routing` section from
   `<kit>/system/AGENTS.md` (from that heading up to, not including, the
   next `## ` heading) and insert it into `<root>/AGENTS.md` just before
   `## Where things go`. If that heading isn't there any more, show the
   user the file's headings and ask where it should go.
3. Do the same for `## 6. Saving tokens on long tasks` from
   `<kit>/system/GETTING-STARTED.md`: append it to the end of
   `<root>/GETTING-STARTED.md`, renumbering it if the user's file has
   more sections. Also add the `agents/` line from section 5 of the kit's
   copy to the user's list of where things live.

Show the user the diff (`git -C "<root>" diff`) before going on.

## 4. Link the subagents

If a file with the same name already exists in `~/.claude/agents`, stop
and ask before replacing it.

```sh
mkdir -p ~/.claude/agents
for a in "<root>"/agents/*.md; do
  ln -s "$a" ~/.claude/agents/"$(basename "$a")"
done
```

## 5. Commit

```sh
git -C "<root>" add -A && git -C "<root>" commit -m "feat: add model routing (dev-kit v0.1.1)"
```

## 6. Test it

1. Check that the links resolve. This should print nothing:
   ```sh
   for l in ~/.claude/agents/*; do [ -e "$l" ] || echo "broken: $l"; done
   ```
2. Ask the user to fully quit Claude Code, open a **new** session in a
   project, and type `/agents`. It should list `scout`, `builder` and
   `analyst`.

Then tell the user they can delete this kit folder again.
