# Upstream Contribution Finder — v2

A Claude Code skill that scans upstream GitHub repos and finds issues/PRs your team
can contribute to. It scores them by workstream relevance, actionability, recency,
and impact — then returns a balanced top 10 across your workstreams. Supports broad
scans (all workstreams) and focused search (describe what you're working on).

Works for any team — reads config from `~/.claude/team-profile.yaml`. On first run,
generates the config automatically from your GitHub activity and CLAUDE.md.

## How it works

1. First run: no config found → onboarding kicks in
2. Onboarding reads your CLAUDE.md + scans your GitHub activity
3. Suggests a team profile (repos, workstreams, keywords)
4. You confirm or edit → saved to `~/.claude/team-profile.yaml`
5. Future runs use that config automatically

## Install

Update symlink to point to v2:

```bash
ln -sfn "/path/to/ai-skills-internal/skills/research-intelligence-feed/v2" ~/.claude/skills/research-intelligence-feed
```

## Run

```
/research-intelligence-feed
```

Or with a focused search:

```
"Find InstructLab issues related to callbacks"
"Show me GRPO and reward modeling issues"
```

## Your team config

The skill saves your team-specific config to:

```
~/.claude/team-profile.yaml
```

This file is NOT in the repo — it lives on your machine only.

### View your config

```bash
cat ~/.claude/team-profile.yaml
```

Or inside Claude Code, say:

```
"Show my profile"
```

### Edit your config

Open it in your editor:

```bash
# VS Code
code ~/.claude/team-profile.yaml

# Vim
vim ~/.claude/team-profile.yaml

# Nano
nano ~/.claude/team-profile.yaml
```

Or regenerate from scratch inside Claude Code:

```
"Update my profile"
```

### Config format

See `team-profile.template.yaml` in this directory for the full format with
examples. The key fields:

```yaml
team: "Your Team Name"
product: "Your Product"

workstreams:
  - name: "Workstream Name"
    repos:
      - org/repo
    keywords:
      - keyword1
      - keyword2

exclude:
  topics: [inference, serving]
  repos: [org-to-skip/*]
  labels: [wontfix]
```

## What changed from v1

| | v1 | v2 |
|---|---|---|
| Team data | Hardcoded in SKILL.md | Lives in ~/.claude/team-profile.yaml |
| First run | Works only for Fine Tuning team | Onboarding generates config for any team |
| Config discovery | None | CLAUDE.md + GitHub activity + user confirmation |
| Profile management | Edit SKILL.md | "Show my profile" / "Update my profile" |
| Repo cleanliness | Team data committed | Only template committed, runtime config in ~/.claude/ |
