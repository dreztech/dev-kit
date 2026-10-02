---
name: curate
description: Move the captured lines in memory/_inbox.md into the memory folder. For each line, add a new fact, update an existing one, skip it, or archive a fact that's now wrong, then rebuild memory/index.md. This is the only way the memory folder gets written. Use when the owner says curate, when resume reports items in the inbox, or after a handoff that queued facts.
---

# curate — tidy the memory inbox

Memory lives in `{{ROOT}}/memory/`. This skill is the only thing that
writes to it. Anything else that wants to remember a fact adds a line to
`memory/_inbox.md` instead.

## Format

Each fact gets its own file, `memory/<short-name>.md`:

```markdown
---
updated: YYYY-MM-DD
---
<The fact in one to three sentences.>
**How to apply:** <when and how this fact should change what an agent does>
```

`memory/index.md` holds one line per fact:
`- [short-name](short-name.md) — <the fact in under 15 words>`.

## Steps

1. **Read** `memory/_inbox.md` and `memory/index.md`.
2. **Decide what to do with each inbox line.** There are four choices:
   - **Add**: it's a new lasting fact. Create a file for it.
   - **Update**: a file already covers it, but the fact has changed.
     Rewrite the file and update its date. If the old version is worth
     keeping, end the file with one line: "Previously: <old fact>
     (until YYYY-MM-DD)."
   - **Skip**: it's already known, or it isn't a lasting fact (task
     progress, finished work, to-dos). Don't write anything.
   - **Archive**: it shows that an existing fact is wrong. Move that file
     to `memory/_archive/`. Never delete a fact file.

   If you aren't sure a fact is true, ask the owner, or leave the line in
   the inbox with `(?)` added to the end. A wrong fact causes more harm
   than a missing one.
3. **Check every file you write.** It must not contain passwords, API
   keys, tokens or other secrets. If an inbox line contains a secret,
   don't copy it anywhere. Tell the owner, and tell them to change that
   password or key, because it has already been written down in plain
   text.
4. **Rebuild `memory/index.md`** so that it lists every file in `memory/`,
   apart from `_inbox.md`, `index.md` and `_archive/`.
5. **Clear the inbox lines you handled.** Read `_inbox.md` again just
   before you change it. The other agent pane may have added a line in the
   meantime. Remove only the lines you dealt with, matching them exactly,
   and leave everything else in place.
6. **Commit** in `{{ROOT}}` with a message like
   `chore(memory): curate — 2 added, 1 updated, 1 archived`.

Report what happened to each inbox line in a single sentence.
