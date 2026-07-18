# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository actually is

Two layers coexist here:

1. **The upstream tutorial** — `NN_Day_*` folders (e.g. `01_Day_Introduction`,
   `23_Day_Event_listeners`) plus language translation folders (`Korea/`, `RU/`,
   `Spanish/`, …). This is Asabeneh's "30 Days Of JavaScript" course, cloned as
   read-only reference material. **Do not modify or "fix" these files** — errors in
   them (e.g. `main.js` relying on variables from a separately-loaded `variable.js`)
   are intentional teaching artifacts, not bugs.

2. **`js_notes/`** — the user's own learning journal and the **active workspace**.
   Nearly all real work happens here. It is organized **by knowledge topic, not by
   day** (`01-basics`, `02-variables`, … `07-objects`, …), because the numbering
   reflects the user's learning order, not the tutorial's day numbers.

The user is a **beginner learning JavaScript** (with some Python/Linux background).
Sessions are interactive tutoring: teach a topic, have them practice, review their code.

## `js_notes/` structure and conventions

- Each topic is a folder with `notes.md` (concepts, human-read) and `practice.js`
  (runnable code the user edits). `js_notes/README.md` is the index + progress map.
- `js_notes/00-concepts/` holds cross-topic concepts (value-vs-reference, call
  forms, bash-vs-js short-circuit) and **`my-mistakes.md`** — a running log of
  mistakes the user has actually made, each with a `📍来源` back-reference to the
  original exercise.

**`js_notes/README.md` contains binding tutoring rules — read it before teaching.**
The critical ones:

- **Never write answers for the user.** In practice files, the "轮到你" (your turn)
  region gets only requirements — no solutions, not even commented-out ones (that
  spoils it). Hints point at direction only ("these two methods can chain"), never a
  full line like `raw.trim().toLowerCase()`.
- **Let the user fix their own code.** Point out what's wrong and why; don't edit
  their exercise answers for them.
- Demonstration regions (teaching examples) may include expected output as comments;
  exercise regions must not.
- **When the user makes a new mistake, absorb it into `00-concepts/my-mistakes.md`**
  (错法→现象→正解→教训 format, with a `📍来源` index) so it's recoverable on review.
- Update the progress checkboxes in `js_notes/README.md` when a topic is finished.

## Running code

There is **no build system, package.json, linter, or test suite** — this is a
learning repo, not an application. Code is run one file at a time:

```bash
node js_notes/06-arrays/practice.js    # run a single practice file
```

Inside the editor the user also runs files via **Quokka.js** (live `console.log`
values inline; free edition — no `//?` live-comments) or **Code Runner** (▷ button).

**Self-contained vs browser files:** a `.js` file that defines everything it uses and
touches no `document` can run under Node/Quokka/Code Runner. Files that depend on
variables from another script, or use `document`/DOM (day 21+ and the mini-projects),
must be opened via their `index.html` in a browser with the F12 console — running them
under Node throws (e.g. `firstName is not defined`, `document is not defined`).

## Content language

Prose, notes, and explanations are in **Chinese**; code, identifiers, and commit
messages in English. Commit messages follow Angular style (`<type>(scope): description`).
