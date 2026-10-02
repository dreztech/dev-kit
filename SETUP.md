# SETUP

**For the agent running this:** follow these steps in order on the user's
Mac. Run commands yourself and show the user what you find. Ask before
doing anything that changes a file outside the new workspace. If a step
fails, stop, explain what went wrong and how to fix it, and don't skip
ahead.

`<kit>` means this repo, the folder you are running in.

## 1. Ask

Ask these together, with a suggested default where there is one:

1. **Your name or handle**, lowercase, such as `sam`. The workspace will
   be called `<name>-dev`.
2. **Where should the workspace go?** The default is `~/<name>-dev`.
3. **Existing projects** to bring in: give a path for each. For each
   one, ask whether to **leave it where it is** (the default, which is
   safest if desktop apps or editors have it open) or **move it into the
   workspace**.
4. **New projects** to start: a name and one line about each. They go
   inside the workspace unless the user says otherwise.

From here on, `<root>` means the workspace's **full path**, such as
`/Users/sam/sam-dev` (use `$HOME`, not `~`, because `~` doesn't expand
inside quotes). `<name>` means the handle.

## 2. Check the machine

```sh
sw_vers -productVersion
git --version
git config --global user.name; git config --global user.email
claude --version
codex --version
command -v herdr
tmutil destinationinfo
```

- **Not macOS:** stop. This kit only supports macOS.
- **No git:** stop. Tell the user to run `xcode-select --install`, then
  run SETUP again.
- **No git name or email:** ask for them, then set them with
  `git config --global user.name "..."` and
  `git config --global user.email "..."`.
- **`claude`, `codex` or `herdr` missing:** carry on, but list what's
  missing in the final summary. The workspace still works with just one
  agent.
- **No Time Machine destination:** carry on, but say so plainly and point
  to step 1 of `GETTING-STARTED.md`. `/resume` will warn about this every
  day until it's fixed.

Also check that `<root>` doesn't exist yet, or is empty. If it already has
files in it, stop and ask. Never write over an existing folder.

## 3. Build the workspace

```sh
mkdir -p "<root>"
cp -R "<kit>/system/." "<root>/"
```

Fill in the placeholders. In these text files, write `{{ROOT}}` with `~`
if the workspace is under the home folder (for example `~/sam-dev`):

- In **every** file under `<root>`: replace `{{OWNER}}` with `<name>` and
  `{{ROOT}}` with `<root>`.
- In `<root>/STATE.md` and `<root>/DECISIONS.md` **only**: replace
  `{{DATE}}` with today's date (`date +%F`).
- Leave `{{PROJECT}}`, `{{DATE}}` and `{{ABOUT}}` alone in
  `<root>/templates/`. The `new-project` skill fills those in later.

Check that nothing is missed. This should print nothing:

```sh
grep -rn '{{OWNER}}\|{{ROOT}}' "<root>"
```

Then make it a repo, with a fresh history that has no connection to the
kit:

```sh
cd "<root>" && git init -b main && git add -A && git commit -m "chore: set up workspace"
```

## 4. Connect both agents

**Skills.** Link each skill folder into both agents' skill folders. If a
skill with the same name already exists in either place, stop and ask
before replacing it.

```sh
mkdir -p ~/.claude/skills ~/.agents/skills
for s in "<root>"/skills/*/; do
  n=$(basename "$s")
  ln -s "${s%/}" ~/.claude/skills/"$n"
  ln -s "${s%/}" ~/.agents/skills/"$n"
done
```

**The rules, everywhere.** Projects can live outside the workspace, so
both agents need to load the workspace `AGENTS.md` wherever they start.
Show the user each change before you make it:

- **Claude Code:** add the line `@<root>/AGENTS.md` to
  `~/.claude/CLAUDE.md`. Create the file if it doesn't exist. If it does,
  add the line at the end and keep what's already there.
- **Codex:** if `~/.codex/AGENTS.md` doesn't exist, link it:
  `mkdir -p ~/.codex && ln -s "<root>/AGENTS.md" ~/.codex/AGENTS.md`. If
  it does exist, add a line at the end:
  `Read and follow <root>/AGENTS.md — it holds this machine's working rules.`

## 5. Bring in the projects

For each project from step 1, follow
`<root>/skills/new-project/SKILL.md`. Existing projects go first, using
the choice made in step 1 (leave in place or move). New projects follow.

## 6. Test it

1. Check that the links resolve. This should print nothing:
   ```sh
   for l in ~/.claude/skills/* ~/.agents/skills/*; do [ -e "$l" ] || echo "broken: $l"; done
   ```
2. Follow `<root>/skills/resume/SKILL.md` in `<root>` and then in one
   project, and show the user both summaries.
3. Ask the user to test the skills themselves:
   - Open a **new** Claude Code session in a project and type `/resume`.
   - If Codex is installed, open it in the same project and type
     `$resume`.

   Both agents should show a summary in the same layout. If either
   doesn't recognise the skill, fully quit that agent and start it again,
   since skills load at startup. Then check the links from step 4 again.

## 7. Finish

Print a short summary:

- the workspace path, and the project list with each project's path
- anything missing from step 2 (agents, herdr, Time Machine) and how to
  fix it
- what to do next: read `<root>/GETTING-STARTED.md`

Tell the user they can delete this kit folder now. Their workspace
doesn't depend on it.
