---
name: readme
description: "Use when writing, rewriting, or reviewing a project README (README.md). Enforces a short, end-user-facing pitch — what the project is, why you'd want it, how to run it — with details pushed out to separate docs. Trigger whenever the user asks to create or improve a README, document a project for its users, or when a new project needs a front page."
---

# READMEs

A README is a sales pitch, not a manual. The reader is deciding whether this
project solves their problem. Answer that in the first ten seconds, show them
how to run it, and link out for everything else.

## Rules

- **Keep it short.** Aim for one screen of prose plus code blocks. If a section
  grows past a handful of lines, it belongs in its own file.
- **Write for the end user**, not for the maintainer. Describe what the project
  does for them. Never list closed issues, internal refactors, roadmap
  bookkeeping, or architecture the user doesn't need to run it.
- **Lead with the pitch.** One or two sentences directly under the title:
  what it is, who it's for, what it replaces. No throat-clearing history.
- **Show, don't spec.** A screenshot, GIF, or a copy-pasteable command beats
  three paragraphs of description.
- **Link out for depth.** Longer material goes in sibling markdown files —
  `CONFIGURATION.md`, `docs/authentication.md`, `CONTRIBUTING.md`,
  `QuickStart.md`, `CHANGELOG.md` — and gets one line in the README.
- **Emojis on headers when they help** scanning (`## Usage 🚀`). Use them
  consistently — all headers or none — and skip them if the project's tone is
  formal. Never put emojis in body prose.

## Shape

Include only the sections the project actually needs. Order matters; drop
anything empty rather than writing filler under it.

```markdown
# <name>

<badges, if any — build, license, version>

<1–2 sentence pitch: what it is and who it's for.>

<screenshot / GIF / short demo block>

## Features ✨
<3–6 bullets, only if the pitch can't carry it alone. User-visible value,
one line each. Not a changelog.>

## Usage 🚀
<Install and run, as copy-pasteable commands. The shortest path from
nothing to working. Longer walkthroughs link to a separate doc.>

## Configuration ⚙️
<The handful of options most users touch — a small table or short list.
Full reference lives in CONFIGURATION.md.>

## Docs 📚
<Links to the detailed markdown files.>

## Contributing 🙋
<One line linking CONTRIBUTING.md.>
```

`Usage` and `Configuration` are the sections worth fighting for — most READMEs
have neither, and they're what a user actually came for.

## Writing the sections

**Pitch.** Name the thing, the category, and the audience:
`Gram is Klarna's threat model diagramming tool — a web app for engineers to
collaboratively document systems as dataflow diagrams.` Concrete nouns, no
adjectives doing the work.

**Usage.** Real commands in fenced blocks, in the order the user runs them.
Numbered steps if there's more than one. State prerequisites only when they'd
actually bite (a system package, a minimum runtime version) — a line or two,
not a section.

**Configuration.** A table of the common knobs:

```markdown
| Option | Default | Description |
| --- | --- | --- |
| `PORT` | `8080` | Port the server listens on. |
```

Anything exhaustive — every env var, auth flows, deployment topologies — goes
in its own file with a pointer from here.

## Reviewing an existing README

Cut, in this order:

1. Sections with no content, or headers that only announce another header.
2. Internal detail: issue numbers, refactor notes, "known limitations" that
   are really a TODO list, historical rationale.
3. Feature lists that restate the code rather than the benefit.
4. Long explanations that should be a linked doc — move them, don't delete
   the content.

Then check the top: does a stranger know what this is and whether they want
it, from the first two sentences alone? If not, rewrite those first.

## Good examples

- [klarna-incubator/gram](https://github.com/klarna-incubator/gram/blob/main/README.md)
  — pitch, screenshot, features, then links out for everything else.
- [nobe4/gh-ln](https://github.com/nobe4/gh-ln/blob/main/README.md) — one-line
  summary, quickstart, "further readings" links.
- [Tethik/launchy](https://github.com/Tethik/launchy/blob/master/README.md) —
  tiny project, tiny README: what it is, preview GIF, deps, install.
