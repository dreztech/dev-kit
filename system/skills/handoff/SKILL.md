---
name: handoff
description: End a work session. Overwrites STATE.md with where things stand and what's next, checks DECISIONS.md, queues facts worth remembering in the memory inbox, and commits. Safe to run more than once, including mid-session as a checkpoint. Use at the end of a session, or when the owner says handoff, wrap up, or save where we are.
---

# handoff — end of session

Only the **writer** pane runs this (see AGENTS.md). If you are the
reviewer, stop and say so.

Running it twice is fine. A second run refreshes STATE and commits
anything new.

## 1. Overwrite STATE.md

Find the nearest `STATE.md`, the same one `resume` uses: start in the
current folder and check each parent folder up to `{{ROOT}}`. Read it
first, then **replace** it completely. Don't add to the old text. Use the
format in AGENTS.md:

- Frontmatter: `updated` (today's date), `status`, and `next` (the single
  most useful next step).
- `## Where we are`: what this session did, where it stopped, and
  anything blocked or waiting on someone.
- `## Next — pick one`: two to four numbered actions. Give each a rough
  size and a reason, and mark the best one `← recommended`. The next
  `resume` shows this list as written.

Only write facts about this project. Write numbers together with where
they came from, for example "14 tests pass (`npm test`)", not a bare "14".

## 2. Check DECISIONS.md

If a choice was made this session and isn't in `DECISIONS.md` yet, add it
now, numbered after the last entry, in the format from AGENTS.md. That
includes a choice that so far only appears in STATE or in a code comment.

## 3. Queue things to remember

Look back over the session for **lasting** facts: a preference the owner
stated, a correction they made, or something about how this machine or a
tool is set up. Don't include task progress, finished work or to-dos.
Those belong in STATE or in a commit.

If there are any, add each one as a single line to
`{{ROOT}}/memory/_inbox.md`, in the form `- YYYY-MM-DD <fact>`. Don't
write to any other file in `memory/`; `/curate` does that later.

## 4. Commit

Commit in every repo this session changed. That includes the project, and
the workspace root `{{ROOT}}` if its files changed (for example
`memory/_inbox.md` or `AGENTS.md`).

1. Run `git status`. Check for files you created that should be committed
   but aren't yet.
2. Check `git diff --staged` for secrets: keys, tokens, passwords, or a
   `.env` file with real values. If you find one, **stop**. Don't commit.
   Tell the owner.
3. Commit with a Conventional Commits message, for example
   `docs(state): handoff — <what the session did>`.

Don't push unless the owner asks you to.

## 5. Write the summary

Keep it short (about 20 lines). Every section appears every time. When a
section has nothing in it, write `— none —`.

```
# handoff — <project>
<date> · <one line on what the session did> · STATE rewritten ✓

## Committed
- <repo> → <hash> <message>   (note unpushed commits; they're fine)

## Remember
<n facts added to memory/_inbox.md — run /curate when convenient | — none —>

## Next — as written to STATE
1. **<action>** — <size>; <why>. ← recommended
2. <action> — <size>; <why>
```
