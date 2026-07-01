# Upstream Contribution Finder

Find upstream contribution opportunities across all Fine Tuning team workstreams.
Scans 7 GitHub repos, scores issues by workstream relevance, and returns a balanced
top 10 — max 3 items per repo, spanning at least 4 workstreams.

## What it does

The skill acts as an **Upstream Contribution Scout** for the Fine Tuning and Training
Hub team within RHOAI Training and Experimentation. It:

1. Queries open issues and PRs across 7 upstream repos in parallel
2. Maps each item to one of 8 team workstreams
3. Filters out operator internals, K8s infra, and non-training topics
4. Scores by workstream match, actionability, recency, and impact
5. Applies balanced selection (max 3 per repo, 4+ workstreams in top 10)
6. Categorizes results: Quick Wins, Feature Contributions, Bug Fixes, Reviews Needed

See [example-output.md](./example-output.md) for a full live report from July 1, 2026.

## Team workstreams

| Workstream | Upstream repos |
|---|---|
| Training Hub Integration | `Red-Hat-AI-Innovation-Team/training_hub` |
| Kubeflow SDK | `kubeflow/trainer`, `kubeflow/training-operator` (migration) |
| InstructLab Training | `instructlab/training` |
| RLVR/GRPO | `huggingface/trl` |
| SFT/LoRA Fine-Tuning | `huggingface/transformers`, `huggingface/peft` |
| KFP Pipelines | Cross-repo pipeline component issues |
| Training Images | Cross-repo container/dependency issues |
| Guided Examples | Notebooks, tutorials, documentation |

## Prerequisites

### Required

**GitHub CLI (`gh`)** — the skill uses `gh issue list` and `gh pr list` directly.

```bash
brew install gh
gh auth login
```

### Optional

**`jq`** — useful if you want to post-process `gh --json` output manually.

```bash
which jq
```

No Python scripts, Jira access, or config files are required. The agent runs the
`gh` commands defined in `SKILL.md` at invocation time.

## Usage

### Via skill invocation

```
/research-intelligence-feed
```

Or ask naturally:

- "Find upstream contribution opportunities"
- "What can we contribute to upstream?"
- "Show me open issues across our workstreams"
- "What's open that's relevant to fine-tuning?"
- "Find issues for the team to pick up"

### Install in Cursor

Copy or symlink `SKILL.md` into your Cursor skills directory:

```
~/.cursor/skills/research-intelligence-feed/SKILL.md
```

Reload Cursor after installing.

### Scheduled runs

```
/loop 24h /research-intelligence-feed
```

## Scoring

**Score = (Workstream Match × 0.35) + (Actionability × 0.30) + (Recency × 0.15) + (Impact × 0.20)**

| Dimension | What boosts the score |
|---|---|
| Workstream Match | Direct mapping to one of the 8 team workstreams |
| Actionability | `good first issue`, `help wanted`, unassigned enhancements/bugs |
| Recency | Created or updated in the last 7–14 days |
| Impact | Reactions, comments, community engagement |

## Output format

Reports are organized into four categories with a coverage summary table:

```markdown
# Upstream Contribution Opportunities — [DATE]

## Quick Wins
## Feature Contributions
## Bug Fixes
## Reviews Needed

### Coverage Summary
| Workstream | Items found | In top 10 |
```

Each item includes repo, workstream, effort estimate, and a link. The coverage
table shows which workstreams had matches and which had gaps.

## Example highlights (July 1, 2026)

From the latest live scan — see [example-output.md](./example-output.md) for the full report:

| # | Item | Workstream |
|---|---|---|
| 1 | Document TrainJob Progress feature (kubeflow/trainer#3553) | Kubeflow SDK |
| 6 | sft() fails on optional params (training_hub#94) | Training Hub |
| 7 | lora_sft() Unsloth padding conflict (training_hub#93) | Training Hub |
| 3 | GRPO-LEAD reward utilities (huggingface/trl#6221) | RLVR/GRPO |
| 10 | LoRA blocked on FP8 checkpoints (transformers#46736) | SFT/LoRA |

That scan covered ~170 issues and 7 PRs across 7 repos. KFP Pipelines and
Training Images had no items in the balanced top 10.

## Troubleshooting

### GitHub rate limits

```bash
gh api rate_limit
gh auth login   # re-authenticate if needed
```

If rate-limited, the skill reduces `--limit` and prioritizes `instructlab/training`
and `huggingface/trl`.

### Repo not accessible

The skill notes inaccessible repos and continues with the rest. It never fails
completely — even one repo's data produces a partial report.

### Empty workstream coverage

Check the coverage summary table at the bottom of the report. Gaps are called out
with suggested broader search terms (e.g. pipeline component, container image, FSDP).

### Skewed results

The max-3-per-repo rule and workstream diversity rule should prevent one repo from
dominating. If results still skew, ask the agent to show the scoring breakdown.

## What's excluded

- Kubeflow Trainer operator internals (controllers, webhooks, reconciliation)
- Kubernetes infrastructure (RBAC, scheduling, PodGroup)
- Inference/serving (not training)
- Midstream ODH repos (`opendatahub-io/*`)
- Internal Jira tickets
- Already-assigned issues (unless labeled help-wanted)
- Stale issues (90+ days inactive, no help-wanted label)

## Contributing

1. Edit `SKILL.md` — repos, keywords, scoring weights, or report template
2. Run the skill against live GitHub data
3. Update `example-output.md` with the new sample report
4. Commit and share with the team

## License

Internal Red Hat tool. Not for public distribution.
