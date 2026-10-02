# Getting started

The whole system on one page.

## 1. Turn on Time Machine (once)

System Settings → General → Time Machine → add a backup disk. Your work
lives on this Mac, so this is your backup. `/resume` warns you if the
last backup is more than a day old.

If `/resume` says it can't read the backup even though Time Machine is
on, give your terminal app Full Disk Access: System Settings → Privacy &
Security → Full Disk Access, then switch your terminal app on.

## 2. Lay out herdr

- **One tab per project.**
- **Two panes per tab:** Claude Code on one side and Codex on the other,
  both opened in the project folder.
- **Pick a writer.** One pane edits and commits. The other is the
  **reviewer**: it reads, checks, plans and answers questions, but never
  edits. Tell each agent its role when you start. You can swap the roles
  whenever you like, but only one pane writes at a time.

## 3. The daily routine

```
/resume   →   work   →   /handoff
```

- **`/resume`** when you sit down. It tells you where you left off,
  checks for problems, and suggests what to do next. Pick a number.
- **Work.** When you make a decision worth remembering, say "log that
  decision" and it goes in `DECISIONS.md`.
- **`/handoff`** before you stop. It writes down where you are, notes
  anything worth remembering, and commits. Only the writer pane runs it.

If you skip a handoff, nothing is lost. Your files are still there.
`/resume` will notice the uncommitted work next time.

## 4. Calling skills

| Skill | Claude Code | Codex | What it does |
|---|---|---|---|
| resume | `/resume` | `$resume` | start of session |
| handoff | `/handoff` | `$handoff` | end of session |
| clarify | `/clarify <request>` | `$clarify <request>` | asks questions before work starts |
| new-project | `/new-project` | `$new-project` | starts or adopts a project |
| curate | `/curate` | `$curate` | tidies memory |

You can also just ask in plain words, for example "let's wrap up" or
"start a new project". The agent will pick the right skill.

## 5. Where things live

- `STATE.md` (one per project): where you are and what's next. Rewritten
  every handoff.
- `DECISIONS.md` (one per project): what you decided and why. Only ever
  added to.
- `memory/`: lasting facts about you and this Mac. Add a line to
  `memory/_inbox.md`, or say "remember that…", and `/curate` files it.
- `AGENTS.md`: the rules both agents follow. Edit it whenever you want
  to change how they work. It's your system now.
