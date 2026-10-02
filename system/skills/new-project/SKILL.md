---
name: new-project
description: Set up a project in this workspace. Either start a brand-new project (create the folder and git repo) or adopt an existing one in place, without touching its history. Adds STATE.md, DECISIONS.md and AGENTS.md, and lists the project in the workspace. Use when the owner wants to start, add, adopt or register a project.
---

# new-project — start or adopt a project

The templates are in `{{ROOT}}/templates/project/`.

## 1. Ask first

Ask only about what you can't already tell:

- **Name**: a short folder-style name, such as `my-site`.
- **New or existing?** If it already exists, get its path.
- **Where it lives:**
  - A new project goes in `{{ROOT}}/<name>/` unless the owner wants it
    somewhere else.
  - An existing project stays where it is unless the owner asks to move
    it. Desktop apps and editors may have its current path saved, and
    moving it breaks those links.
- **One line on what it is.** This goes in the project's AGENTS.md.

## 2. Check before changing anything

For an **existing** project:

- Run `git -C <path> status --short`. If the project isn't a git repo
  yet, offer to run `git init -b main` and make a first commit of the
  current files. If it has uncommitted work, stop and ask the owner to
  commit or stash it first.
- If it already has a `STATE.md`, `DECISIONS.md`, `AGENTS.md` or
  `CLAUDE.md`, **don't overwrite it.** Show the owner what's there, then
  merge (for example, add the pointer section to an existing AGENTS.md),
  or skip that file.

If the owner asked to **move** an existing project:

- Its tree must be clean first.
- Use `mv <old> {{ROOT}}/<name>`. Git history moves with the folder.
- Remind the owner to reopen it from the new location in their desktop
  apps and editor.

## 3. Create the files

For a **new** project, start with
`mkdir -p <path> && git -C <path> init -b main`.

Copy each template into the project, skipping any that step 2 said to
keep. Replace `{{PROJECT}}` with the name, `{{DATE}}` with today's date
(`date +%F`), and `{{ABOUT}}` with the one-line description:

| Template | Becomes |
|---|---|
| `templates/project/AGENTS.md` | `<path>/AGENTS.md` |
| `templates/project/CLAUDE.md` | `<path>/CLAUDE.md` |
| `templates/project/STATE.md` | `<path>/STATE.md` |
| `templates/project/DECISIONS.md` | `<path>/DECISIONS.md` |

For an existing project, fill in STATE's "Where we are" from what you can
see: the last few commits (`git log --oneline -10`) and the README. Say
that this is a first reading for the owner to correct.

## 4. Register it in the workspace

1. Add a line under the project list in `{{ROOT}}/AGENTS.md`:
   `- **<name>** — <one line> · \`<path>\``.
2. If the project is inside `{{ROOT}}`, add `/<name>/` to
   `{{ROOT}}/.gitignore`. It has its own repo, and the workspace repo must
   not track it.

## 5. Commit

- In the project: `chore: add STATE, DECISIONS and AGENTS` (or
  `chore: initial commit` for a new, empty project).
- In `{{ROOT}}`: `chore: add project <name>`.

Finish by running the `resume` checks inside the project, to show that
everything works.
