---
name: trufflehog
description: Secret scanning skill for pentesters using TruffleHog. Use this skill whenever the user wants to scan repositories for leaked secrets, credentials, API keys, tokens, or sensitive data. Triggers on local repo paths or GitHub URLs. Use it when the user says things like "scan this repo for secrets", "run trufflehog", "check for leaked credentials", "look for API keys", "secret discovery", or "check this GitHub repo". Even if the user doesn't explicitly say "trufflehog" but is doing recon or secret hunting on a repo, use this skill.
---

# TruffleHog Secret Scanner

You are helping a pentester scan one or more repositories for leaked secrets using TruffleHog.

## Step 1: Ensure TruffleHog is installed

```bash
which trufflehog
```

If not found, install it (Linux/macOS):
```bash
curl -sSfL https://raw.githubusercontent.com/trufflesecurity/trufflehog/main/scripts/install.sh | sh -s -- -b /usr/local/bin
```

Verify: `trufflehog --version`

## Step 2: Identify targets

Parse the user's request for:
- **Local paths** — absolute or relative paths to git repos on disk
- **GitHub repo URLs** — `https://github.com/org/repo`
- **GitHub org** — `https://github.com/org` or just an org name (no specific repo)

If ambiguous (e.g., just an org name), ask whether they want all repos or specific ones.

## Step 3: Run the scan

Pick the right command per target:

**Local repo:**
```bash
trufflehog git file:///absolute/path/to/repo --results=verified --json 2>/dev/null
```

**GitHub repo URL:**
```bash
trufflehog git https://github.com/org/repo --results=verified --json 2>/dev/null
```

**GitHub org (all repos — use with care on large orgs):**
```bash
trufflehog github --org=<orgname> --results=verified --json 2>/dev/null
```

### Saving output

TruffleHog's `--json` emits one JSON object per line (newline-delimited). Capture it with `tee` so it prints live and saves simultaneously:

```bash
trufflehog git <target> --json 2>/dev/null | tee trufflehog-<repo-name>-<YYYY-MM-DD>.json
```

Name the output file after the repo and today's date, e.g. `trufflehog-security-infra-2026-06-05.json`.

For multiple targets, scan each separately and save to separate files, then tell the user all the file paths at the end.

### Performance note

TruffleHog scans the full git history by default, which can be slow on large repos. If the user wants a faster scan, suggest:
- `--since-commit <hash>` to scan from a specific commit
- `--branch <name>` to limit to one branch
- `--only-verified` to skip unverified findings (faster, fewer false positives)

## Step 4: Parse and summarize results

After each scan completes, read the output JSON and produce a summary:

```
=== TruffleHog Results: <repo-name> ===
Total findings:   12
  Verified:       3
  Unverified:     9

Top detector types:
  GitHubToken     5
  AWS             4
  GCP             3

High-value findings (verified):
  [GitHubToken] File: .env | Commit: abc123 | Author: Jane Doe <jane@example.com>
  [AWS]         File: config/prod.yml | Commit: def456 | Author: ...
  [GCP]         File: scripts/deploy.sh | Commit: ...

Output saved to: trufflehog-<repo-name>-2026-06-05.json
```

Show full detail for verified findings. For unverified, just show counts by type unless the user asks for more.

Flag particularly sensitive detector types for the user's attention: `GitHubToken`, `AWS`, `GCP`, `Slack`, `Stripe`, `Twilio`, `PrivateKey`.

## Step 5: Wrap up

Tell the user:
1. Where each JSON file was saved (absolute path)
2. Total verified vs unverified finding counts across all targets
3. Any next steps worth considering (e.g., reporting, revocation)
