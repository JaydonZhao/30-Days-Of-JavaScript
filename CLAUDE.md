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

- **One file per topic**, flat at the `js_notes/` root: `NN-topic.js` (e.g.
  `09-loops.js`). Each file is self-contained "learn + practice + review" in three
  labelled regions, in this order:
  1. `// ═════ 概念总结 ═════` — concepts as comments (with aligned ASCII tables)
  2. `// ═════ 示范 ═════` — runnable demo code, may carry expected-output comments
  3. `// ═════ 轮到你 ═════` — exercises: requirements only, the user writes the code
  `js_notes/README.md` is the index + progress map.
- **History:** topics used to be `NN-topic/` folders holding `notes.md` +
  `practice.js`; they were flattened into single `NN-topic.js` files. Some `📍来源`
  links inside `my-mistakes.md` still point at the old `../NN-topic/practice.js`
  paths — treat those as `../NN-topic.js` (fix a link only if you're already editing
  that entry; don't do a blanket rewrite of the user's file).
- `js_notes/00-concepts/` stays a **folder** of cross-topic `.md` notes
  (value-vs-reference, call-forms, bash-vs-js short-circuit, semicolons-and-asi) plus
  **`my-mistakes.md`** — a running log of mistakes the user has actually made, each in
  错法→现象→正解→教训 format with a `📍来源` back-reference to the original exercise.

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
  But **only genuinely instructive mistakes** — a plain typo already covered by an
  existing entry, or one the user already fixed with nothing new to learn, is not
  worth a new entry (the user will push back if you log noise). When unsure, ask.
- Update the progress checkboxes in `js_notes/README.md` when a topic is finished.

### Leave learning traces when editing the user's code (user's standing request)

Whenever the user asks you to change their code or add markers, **preserve the wrong
version as a commented `❌ / ✅` contrast** right next to the fix — so future-them sees
*what was wrong and why*, not just the corrected line. This is the same style the user
already uses in `06-arrays.js` / `07-objects.js`:

```js
// ❌ splice 返回的是"被删除的元素",不是修改后的数组
// console.log(q.splice(1, 0, 'second'))
// ✅
q.splice(1, 0, 'second')
console.log(q)
```

Rules for these traces:
- Comment out the wrong line (never leave broken code live), keep it **above** the fix.
- One short `// ❌ 原因` line saying *why* it's wrong; `// ✅` marks the correct code.
- This applies to **demo/teaching regions and to fixes the user explicitly asks for** —
  it does **not** override "never write answers": still don't drop solutions into an
  untouched `轮到你` exercise the user hasn't attempted.
- A substantial new trap that emerges this way should usually also get a
  `my-mistakes.md` entry (subject to the "instructive only" bar above).

### Explain the *why*, not just the rule (user's standing request)

The user learns far faster from the underlying mechanism than from a bare
"that's just how it is." Terse rule-only notes are not enough — the user
explicitly flagged `// 带 {} 就得自己写 return` as 晦涩 (opaque). When a rule
has a reason, **lead with the reason**; once the mechanism is clear the rule
becomes self-evident (and often several rules collapse into one cause).

Worked example — arrow-function body & `return`. Lead with the **anchor
sentence**, then let everything fall out as consequences (finding the right
anchor is the skill — the user corrected an earlier draft that started from
`{`'s two identities instead of from the anchor):
- **Anchor: the right side of `=>` expects an *expression*** (something that
  evaluates to a value).
- Give it an expression → that *is* the value → JS returns it (implicit return).
  `return` there is illegal: `return` is a *statement*, not an expression, so
  `(a, b) => return a + b` is a `SyntaxError`.
- `{ ... }` is the escape hatch: starting the body with `{` makes JS treat it as
  a *statement block*, which produces no value → you must `return` explicitly,
  else `undefined`.
- Corollary from the same anchor: an object literal also starts with `{`, so it
  collides with the block rule → wrap in parens to force expression context:
  `(x) => ({ name: x })`.

Practical habits this implies:
- Prefer "X **because** the parser/engine sees Y" over "always do X."
- **Verify by running code** when it sharpens the point — the user values seeing
  the actual error message or output over an assertion (e.g. `node --check` to
  show a real `SyntaxError`).

## Running code

There is **no build system, package.json, linter, or test suite** — this is a
learning repo, not an application. Code is run one file at a time:

```bash
node js_notes/06-arrays.js       # run a single topic file
node --check js_notes/09-loops.js  # syntax-check only, no execution
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

## Git remotes

This clone was forked away from the original course:

- `origin` → `github.com/JaydonZhao/30-Days-Of-JavaScript` (the user's fork; **push here**)
- `upstream` → `github.com/Asabeneh/30-Days-Of-JavaScript` (original author; pull tutorial
  updates with `git fetch upstream && git merge upstream/master` — merge, not rebase)

Work happens on `master` directly (the fork's `master` is independent of upstream's, so
the user's commits never touch anyone else's repo). The fork is **public** — don't commit
anything private.
