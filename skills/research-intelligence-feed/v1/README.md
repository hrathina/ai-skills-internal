# Upstream Contribution Finder (`research-intelligence-feed`)

Find upstream contribution opportunities across Fine Tuning team workstreams. The skill scans GitHub repos, scores issues by relevance, and returns a balanced top 10 — or a focused list when you describe what you want to work on.

See [example-output.md](./example-output.md) for sample reports (broad scan and focused search).

No GitHub CLI, Jira access, or config files required — just install the skill and run it.

## Clone the repo

```bash
git clone https://github.com/hrathina/ai-skills-internal.git
cd ai-skills-internal
```

## Install the skill (any agent)

The skill is a single file: `SKILL.md`. Copy or symlink the skill directory into your agent's skills folder.

```bash
# From this repo
SKILL_DIR="skills/research-intelligence-feed/v1"
```

### Cursor

**Personal (all projects):**

```bash
mkdir -p ~/.cursor/skills/research-intelligence-feed
cp SKILL.md ~/.cursor/skills/research-intelligence-feed/SKILL.md
```

Or symlink so updates from the repo are picked up automatically:

```bash
ln -sf "$(pwd)/$SKILL_DIR/SKILL.md" ~/.cursor/skills/research-intelligence-feed/SKILL.md
```

**Project-only:** copy or symlink into `.cursor/skills/research-intelligence-feed/SKILL.md` in your workspace.

Reload Cursor after installing. The skill appears when you type `/research-intelligence-feed` or ask for upstream contribution opportunities.

### Claude Code

```bash
mkdir -p ~/.claude/skills/research-intelligence-feed
cp SKILL.md ~/.claude/skills/research-intelligence-feed/SKILL.md
```

Or symlink:

```bash
ln -sf "$(pwd)/$SKILL_DIR/SKILL.md" ~/.claude/skills/research-intelligence-feed/SKILL.md
```

Restart Claude Code or start a new session. Invoke by name or describe what you want (see Usage below).

### Other agents (Codex, Gemini CLI, custom setups)

Any agent that loads markdown skill/instruction files from a directory can use this skill:

1. Locate where your agent reads skills (check that product's docs for `SKILL.md` or `skills/` paths).
2. Copy or symlink `SKILL.md` into a folder named `research-intelligence-feed`.

If your agent supports attaching skills per message, you can attach `SKILL.md` directly without installing.

## Usage

### Broad scan (default)

Scans all 7 upstream repos and returns a balanced top 10 across workstreams.

```
/research-intelligence-feed
```

Or ask naturally:

- "Find upstream contribution opportunities"
- "What can we contribute to upstream?"
- "Show me open issues across our workstreams"

### Focused search

Narrow to a repo, workstream, or topic:

```
/research-intelligence-feed I would like to contribute to Kubeflow Trainer
```

```
/research-intelligence-feed I'm working on speculative training
```

```
/research-intelligence-feed Show me GRPO and reward modeling issues
```

Focused mode queries only relevant repos and uses your description as search keywords.

## Upstream repos scanned

| Repo | Workstream |
|---|---|
| `Red-Hat-AI-Innovation-Team/training_hub` | Training Hub Integration |
| `kubeflow/trainer` | Kubeflow SDK |
| `kubeflow/training-operator` | Kubeflow SDK (migration) |
| `instructlab/training` | InstructLab Training |
| `huggingface/trl` | RLVR/GRPO |
| `huggingface/transformers` | SFT/LoRA Fine-Tuning |
| `huggingface/peft` | SFT/LoRA Fine-Tuning |

## Picking an issue to work on

1. Run the skill and review the top 10.
2. Some issues may already be assigned or in active discussion — the report filters most of these, but double-check on GitHub.
3. If you want to contribute to an issue, **post it in the team channel** with the link and a short note on why it fits.
4. Wait for team feedback before starting — others may know context (already in progress, wrong workstream, needs upstream maintainer buy-in).

## Output format

Reports are grouped into:

- **Quick Wins** — good first issues, docs, small fixes
- **Feature Contributions** — enhancements and new capabilities
- **Bug Fixes** — bugs in training/fine-tuning codepaths
- **Reviews Needed** — open PRs worth reviewing
- **Already In Progress** — assigned issues for awareness (not for pickup)

Each item includes repo, workstream, RHOAI impact, effort tag, and link. A coverage summary table shows which workstreams had matches and which had gaps.

## Troubleshooting

**Skill not appearing**

Reload your agent after install (Cursor: restart or new window; Claude Code: new session). Confirm `SKILL.md` is in the correct skills folder.

**Empty or partial results**

The agent queries GitHub on your behalf. If some repos fail, the report continues with whatever data is available and notes gaps in the coverage summary.

## Updating the skill

Pull the latest from `ai-skills-internal` and re-copy or refresh your symlink:

```bash
git pull
# Re-run the cp or ln -sf command from the Install section above
```

## Contributing

1. Edit `SKILL.md` — repos, keywords, scoring, or report template
2. Test against live GitHub data
3. Update `example-output.md` with a sample report
4. Open a PR in `ai-skills-internal`
