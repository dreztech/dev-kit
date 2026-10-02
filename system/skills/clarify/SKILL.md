---
name: clarify
description: Ask questions about a request until no remaining answer would change what gets built, then restate the request and wait for go. Builds and edits nothing. Use when the owner says clarify, starts a request with clarify, or asks to be questioned before work starts.
---

# clarify — understand the request before building

**This skill builds nothing and edits nothing.** It ends with a
restatement of the request, then waits for "go".

## Starting it

- `clarify <request>`: the request is what follows.
- `clarify` with nothing after it: the request is the owner's previous
  message.
- `fast` anywhere in the request: ask one round of at most four
  questions, then restate.

## Stance

Your job is to understand what the owner wants, not to argue for
something else. If you see a real problem, or a much simpler way to do
it, say so in **one line** that starts with `⚑`, then carry on.

## When to stop

Stop asking once **no remaining question would change what gets built**.
Anything still unknown must go in one of two groups:

- **Assumption**: a default you will go ahead with unless told otherwise.
  The owner can correct it in a line.
- **Found out by doing**: something only the work itself will reveal. No
  question asked now would settle it.

## What to ask about

Work through these in order. **Skip anything the request already
answers.** Before asking, read the project's files and `STATE.md`, so you
don't ask about anything they already answer.

1. **Goal**: what "done" looks like, and how the owner will tell that it
   worked.
2. **Edges**: what's in, what's out, and the smallest version that would
   still do the job.
3. **Constraints**: the tools, the files and conventions to follow, any
   deadline, and anything that mustn't be touched.
4. **Output**: who it's for and what form it takes: code, an answer in
   chat, a file, a document.
5. **The undecided part.** *Never skip this one.* The owner uses this
   skill when they haven't settled what they want yet. Find the part they
   haven't decided, name it, and ask about it directly. This is usually
   the question that matters most.

## How to ask

- Ask at most four questions per round. Put a recommended default first
  in each question and label it "(recommended)", so that answering is
  mostly picking. If your agent has a multiple-choice question tool, use
  it. Otherwise, number the questions and letter the options
  (`1a`, `2c`) so the owner can reply in a few keystrokes.
- Only ask questions together in a round if their answers don't depend
  on each other.
- Ask at most three rounds. After that, stop asking and restate the
  request. Anything still open becomes an Assumption.
- Read each answer literally. An answer may change the request, ask you
  a question back, or say stop. Do what it actually says.

**Don't:**
- ask filler questions to look thorough;
- ask anything the request, the files or STATE already answers;
- ask about something with an obvious standard default (list the default
  as an Assumption instead);
- ask about implementation details the owner won't care about.

## The restatement

```
## Clarified
**Build:** <2–5 lines: what it is, who it's for, what form it takes>
**Not building:** <what's left out, or — none —>
**Assumptions (correct any line):**
- <default 1>
- <default 2>
**Found out by doing:** <items, or — none —>
⚑ <a one-line concern, only if there is a real one>

Say **go**, or correct a line.
```

Then **stop**. If the owner corrects a line, revisit only that point,
not the whole list. "Go" means start the work now, as restated.
