---
name: resume
description: Start a work session. Reads the nearest STATE.md, checks git, backup and the memory inbox, then suggests the next few actions to choose from. Read-only. Use at the start of a session, or when the owner says resume, where were we, or what's next.
---

# resume — start of session

**This skill only reads. It changes nothing.**

## 1. Find the STATE

Look in the current folder for `STATE.md`. If it isn't there, check each
parent folder in turn, stopping at `{{ROOT}}`. The first one found is the
STATE for this session. Read all of it. If none is found, say so and offer
to set the folder up as a project with `new-project`.

## 2. Run the checks

Run these from the folder that holds that STATE. Report only what the
commands actually print. If a command fails, say it failed. Never report a
check as passing if it didn't run.

```sh
git status --short --branch
git log -1 --format='%cr · %h %s'
git rev-list --count @{u}..HEAD 2>/dev/null || echo "no upstream"
```

- **Uncommitted files:** warn. The last session probably ended without a
  handoff. Name the files.
- **Unpushed commits:** mention them, but don't warn. They're safe on
  disk.

**Backup (Time Machine):**

```sh
ts=$(tmutil latestbackup 2>/dev/null | grep -oE '[0-9]{4}-[0-9]{2}-[0-9]{2}-[0-9]{6}' | tail -1)
if [ -z "$ts" ]; then echo "no backup found"; else
  age=$(( ( $(date +%s) - $(date -j -f %Y-%m-%d-%H%M%S "$ts" +%s) ) / 3600 ))
  echo "last backup $ts (${age}h ago)"
fi
```

- Over 24 hours ago: warn.
- "no backup found": warn. Either Time Machine is off, the backup disk
  hasn't been connected, or the terminal app lacks Full Disk Access
  (System Settings → Privacy & Security → Full Disk Access). Say which of
  these is likely and how to fix it.

**Memory inbox:** count the lines starting with `- ` in
`{{ROOT}}/memory/_inbox.md`. If there are any, suggest running `/curate`.

## 3. Write the summary

Use exactly this layout, and keep it short (about 25 lines). Every section
appears every time. When a section has nothing in it, write `— none —`.
Be direct: tell the owner what to do, not just what the situation is.

```
# resume — <project>
STATE updated <date> · <status>

## Checks
<one line if everything is fine: branch · clean/dirty · last commit age · backup age · inbox count>
⚠ <one line for each problem>

## Where we are
<up to 5 lines from STATE: what's in progress, what changed last time, anything blocked>

## Next — pick one
1. **<action>** — <rough size>; <why>. ← recommended: <one-sentence reason>
2. <action> — <rough size>; <why>
3. <action> — <rough size>; <why>
```

If the STATE already has a `## Next — pick one` list, reuse it. Only change
the order if a check turned up something more urgent, such as uncommitted
work or a stale backup.

Then stop and wait for the owner to choose.

If you are the reviewer pane (see AGENTS.md), say so on the line under the
title. The work you can offer is reviewing, planning and answering
questions, not editing.
