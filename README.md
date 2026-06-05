# skills

Skills for agents (e.g. Claude Code) to use.

Each skill lives in its own directory containing a `SKILL.md` file with YAML
frontmatter (`name`, `description`) followed by the instructions the agent
follows when the skill is triggered.

## Available skills

| Skill | Description |
| --- | --- |
| [`trufflehog`](trufflehog/SKILL.md) | Secret scanning for pentesters using [TruffleHog](https://github.com/trufflesecurity/trufflehog). Scans local repos, GitHub repo URLs, or whole GitHub orgs for leaked secrets, credentials, API keys, and tokens, then summarizes verified vs. unverified findings. |

## Installing a skill

Skills are loaded from `~/.claude/skills/`. To install one, copy its directory
there (the directory name should match the skill `name`):

```bash
git clone https://github.com/<org>/skills.git
cp -r skills/trufflehog ~/.claude/skills/trufflehog
```

Restart your agent (or start a new session) and the skill becomes available —
it triggers automatically when your request matches its `description`, or you
can invoke it explicitly with `/trufflehog`.
