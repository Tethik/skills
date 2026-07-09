---
name: github-pull-requests
description: "Use when committing changes, writing commit messages, or opening/updating GitHub pull requests with git and the `gh` CLI. Enforces short, concise commit messages and PR descriptions, no test-plan sections, and draft-by-default PR creation. Trigger whenever the user asks to commit, push, open a PR, or when finishing a change that should land on GitHub."
---

# GitHub Pull Requests

House style for commits and pull requests. Keep everything short. Prose costs review time.

## Commits

- One logical change per commit. Branch off the base branch first if on it.
- Subject line: imperative mood, ≤ 50 chars, no trailing period (`Add retry to fetch client`).
- Body: only if the *why* isn't obvious from the subject. A sentence or two, wrapped ~72 cols. Skip it otherwise — no filler.
- Never restate the diff line by line. The diff already says what changed.

## Pull requests

**Always create as a draft by default** — use `gh pr create --draft`. Only skip `--draft` when the user explicitly asks to open it ready for review.

PR title: same style as a commit subject — concise, imperative, no period.

PR body: keep it to a few lines. Use this shape, dropping any section that adds nothing:

```markdown
## Summary
<1–3 sentences: what changed and why.>

## Notes
<Optional. Anything a reviewer genuinely needs: risk, follow-ups, links. Omit if none.>
```

### Do NOT include

- **No "Test plan" / "Testing" / "How to test" / "Steps to reproduce" section.** Leave it out entirely.
- No checklists, no screenshots-required boilerplate, no ceremony headings with empty content.
- No line-by-line changelog of the diff.

### Creating the PR

```sh
git push -u origin <branch>
gh pr create --draft --title "<concise title>" --body "<concise body>"
```

Mark ready only when asked: `gh pr ready <number>`.
