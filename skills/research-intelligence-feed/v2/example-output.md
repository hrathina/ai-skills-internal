# Example Outputs

Two sample reports showing both modes: a broad scan across all workstreams and a
focused search narrowed to speculative training.

---

## Example 1: Broad Scan

# Upstream Contribution Opportunities — July 1, 2026

> Issues and PRs across Fine Tuning team workstreams. Max 3 per repo, balanced
> across workstreams.

## Quick Wins

### 1. Document how to use TrainJob Progress feature
- **Repo**: kubeflow/trainer#3553
- **Workstream**: Kubeflow SDK
- **Why contribute**: Labeled good first issue and help wanted — documents the TrainJob Progress API the team exposes through the Kubeflow SDK. Good entry point for SDK contributors.
- **Effort**: Small — docs/config
- **Link**: https://github.com/kubeflow/trainer/issues/3553

## Feature Contributions

### 2. Support GRPO-style post-training in Kubeflow Trainer via TrainingRuntime
- **Repo**: kubeflow/trainer#3508
- **Workstream**: Kubeflow SDK
- **What's needed**: Add TrainingRuntime and Torch plugin support for GRPO-style post-training jobs.
- **RHOAI impact**: Directly aligns with the team's GRPO/RLVR integration path into RHOAI via Kubeflow TrainJob and algorithm registry.
- **Effort**: Large — needs upstream discussion
- **Link**: https://github.com/kubeflow/trainer/issues/3508

### 3. Reasoning reward utilities: GRPO-LEAD + graduated format reward
- **Repo**: huggingface/trl#6221
- **Workstream**: RLVR/GRPO
- **What's needed**: Add group-relative length-scaled accuracy (GRPO-LEAD) and graduated format reward utilities for reasoning tasks.
- **RHOAI impact**: Reward shaping utilities the team can use in GRPOTrainer-based LoRA GRPO and full-weight GRPO backends.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/huggingface/trl/issues/6221

### 4. Discussion: testing strategy for instructlab/training as a backend library
- **Repo**: instructlab/training#705
- **Workstream**: InstructLab Training
- **What's needed**: Define a testing strategy for using instructlab/training as an embeddable backend library (the team's SFT path).
- **RHOAI impact**: The team consumes instructlab/training as the SFT backend — a clear testing contract reduces integration risk in Training Hub.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/instructlab/training/issues/705

### 5. Create templated training notebooks for supported algorithms
- **Repo**: Red-Hat-AI-Innovation-Team/training_hub#69
- **Workstream**: Guided Examples
- **What's needed**: Notebook templates covering each supported Training Hub algorithm (SFT, OSFT, LoRA SFT, GRPO, etc.).
- **RHOAI impact**: Directly feeds RHOAI guided examples and release documentation for each algorithm.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/Red-Hat-AI-Innovation-Team/training_hub/issues/69

## Bug Fixes

### 6. sft() fails: TrainingArgs requires data_output_dir and max_batch_len but API marks them optional
- **Repo**: Red-Hat-AI-Innovation-Team/training_hub#94
- **Workstream**: Training Hub Integration
- **Impact**: sft() convenience function breaks when optional parameters are omitted — affects the primary SFT entry point the team ships.
- **Effort**: Small — docs/config
- **Link**: https://github.com/Red-Hat-AI-Innovation-Team/training_hub/issues/94

### 7. lora_sft() fails with padding_free/packing conflict on Unsloth 2026.6.1
- **Repo**: Red-Hat-AI-Innovation-Team/training_hub#93
- **Workstream**: Training Hub Integration
- **Impact**: LoRA SFT path breaks on current Unsloth — blocks the team's primary LoRA fine-tuning workflow.
- **Effort**: Small — docs/config
- **Link**: https://github.com/Red-Hat-AI-Innovation-Team/training_hub/issues/93

### 8. GRPOTrainer.evaluate() with environment_factory crashes on partial batches
- **Repo**: huggingface/trl#6129
- **Workstream**: RLVR/GRPO
- **Impact**: Evaluation path fails for agent/environment-based GRPO — relevant to team's GRPO rollout and reward integration work.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/huggingface/trl/issues/6129

### 9. AdaLora's update_and_allocate method is lost in inject_adapter_in_model()
- **Repo**: huggingface/peft#3373
- **Workstream**: SFT/LoRA Fine-Tuning
- **Impact**: AdaLoRA rank pruning silently broken after adapter injection — affects LoRA adapter workflows the team uses via PEFT.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/huggingface/peft/issues/3373

### 10. LoRA fine-tuning of an FP8 checkpoint is blocked
- **Repo**: huggingface/transformers#46736
- **Workstream**: SFT/LoRA Fine-Tuning
- **Impact**: get_peft_model does not clear validate_quantization_for_training for FP8 checkpoints — blocks LoRA on quantized models the team may target in training images.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/huggingface/transformers/issues/46736

## Reviews Needed

No training-relevant open PRs found this scan. The 7 open PRs in instructlab/training are dependency and CI/CD bumps only.

## Already In Progress

> Assigned issues relevant to the team — not for pickup, but good to be aware of. Max 3.

_No high-relevance assigned issues surfaced this scan._

---

### Coverage Summary

| Workstream | Items found | In top 10 |
|---|---|---|
| Training Hub Integration | 12 | 2 |
| Kubeflow SDK | 8 | 2 |
| InstructLab Training | 3 | 1 |
| RLVR/GRPO | 28 | 2 |
| SFT/LoRA Fine-Tuning | 22 | 2 |
| KFP Pipelines | 0 | 0 |
| Training Images | 2 | 0 |
| Guided Examples | 3 | 1 |

Scanned: ~170 issues and 7 PRs across 7 repos
Generated: July 1, 2026

**Gaps this scan:** No KFP pipeline or training-image issues made the balanced top 10. kubeflow/training-operator returned no SDK/migration issues. Consider broader searches for pipeline component, container image, and FSDP if those workstreams need coverage next run.

---

## Example 2: Focused Search — Speculative Training

# Upstream Contribution Opportunities — July 2, 2026

> Focus: Speculative training (Eagle3 draft models, hidden-state extraction,
> acceptance-rate validation). Max 3 per repo, balanced where possible.

## Start Here — Your Workstream

If you're picking up speculative / Eagle3 draft-model training, this is the anchor
issue on the team side:

**Red-Hat-AI-Innovation-Team/training_hub#86** — Add Eagle3 speculative decoding
draft model training (speculators)

It defines the two-step workflow you're implementing:

- `generate_responses()` — verifier runs via vLLM to produce (prompt, response)
  training pairs
- `eagle3_train()` — vLLM in the training loop, streaming hidden states in-memory
  to the speculators trainer (same architectural pattern as `lora_grpo_verl.py`)

Open questions called out in the issue that are worth tackling first: vLLM
hidden-state extraction API stability, GPU memory partitioning between verifier TP
and draft trainer, and vocab mapping for deployment artifacts.

## Quick Wins

### 1. Fix hidden-state corruption in CUDA graph padding path
- **Repo**: vllm-project/vllm#43810
- **Workstream**: Training Hub Integration (upstream dependency)
- **Why contribute**: `extract_hidden_states` is the offline path for Eagle3/MTP draft training data. CUDA graph padding writes `PADDING_SLOT_ID=-1` into real KV slots, zeroing hidden states — directly corrupts training inputs for `eagle3_train()`. Includes a minimal repro and one-line fix.
- **Effort**: Small — docs/config
- **Link**: https://github.com/vllm-project/vllm/issues/43810

## Feature Contributions

### 2. Add Eagle3 speculative decoding draft model training (speculators)
- **Repo**: Red-Hat-AI-Innovation-Team/training_hub#86
- **Workstream**: Training Hub Integration
- **What's needed**: Implement `generate_responses()` + `eagle3_train()` as Training Hub algorithms with vLLM + speculators backends.
- **RHOAI impact**: This is the team's managed workflow for custom draft models on fine-tuned verifiers — the feature you're actively building.
- **Effort**: Large — needs upstream discussion
- **Link**: https://github.com/Red-Hat-AI-Innovation-Team/training_hub/issues/86

### 3. Response-regeneration refactor — resumable, quality-filtered training data
- **Repo**: vllm-project/speculators#584
- **Workstream**: Training Hub Integration
- **What's needed**: Refactor `scripts/response_regeneration/script.py` for stable resume IDs, multi-split output, quality filtering (drop truncated/user-only rows), and robust vLLM retries.
- **RHOAI impact**: Direct upstream counterpart to Training Hub's `generate_responses()` step — fixing this upstream reduces duplicate work and improves draft-training data quality for Eagle3.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/vllm-project/speculators/issues/584

### 4. Extend hidden-state extraction to export verifier KV caches
- **Repo**: vllm-project/vllm#45175
- **Workstream**: Training Hub Integration
- **What's needed**: RFC to export verifier attention KV caches alongside hidden states in the extraction pipeline (companion to speculators#586).
- **RHOAI impact**: Eagle3 is self-contained via hidden states, but Gemma4 MTP and future draft architectures need verifier KV tensors as training inputs — blocks the next wave of speculative training support.
- **Effort**: Large — needs upstream discussion
- **Link**: https://github.com/vllm-project/vllm/issues/45175

### 5. Gemma4 MTP draft-model training support
- **Repo**: vllm-project/speculators#586
- **Workstream**: Training Hub Integration
- **What's needed**: Extend speculators data contract with `verifier_kv_last_local/global`, add query-only attention module and multi-level LM head.
- **RHOAI impact**: Extends the draft-training stack beyond Eagle3 — same vLLM extraction + speculators training pattern your Eagle3 work establishes.
- **Effort**: Large — needs upstream discussion
- **Link**: https://github.com/vllm-project/speculators/issues/586

### 6. Create templated training notebooks for supported algorithms
- **Repo**: Red-Hat-AI-Innovation-Team/training_hub#69
- **Workstream**: Guided Examples
- **What's needed**: Notebook templates for SFT, OSFT, LoRA SFT with memory-estimating hyperparameters.
- **RHOAI impact**: Natural place to add an Eagle3 speculative-training notebook once #86 lands — gives users a guided entry point for draft-model training.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/Red-Hat-AI-Innovation-Team/training_hub/issues/69

### 7. Support GRPO-style post-training via TrainingRuntime
- **Repo**: kubeflow/trainer#3508
- **Workstream**: Kubeflow SDK
- **What's needed**: GRPO-focused TrainingRuntime manifests and Torch plugin support.
- **RHOAI impact**: Closest existing Training Hub pattern to Eagle3 online training — `lora_grpo_verl.py` runs vLLM rollout workers alongside FSDP training workers, same co-resident GPU pattern as `eagle3_train()`.
- **Effort**: Large — needs upstream discussion
- **Link**: https://github.com/kubeflow/trainer/issues/3508

## Bug Fixes

### 8. Hidden state / token_ids length mismatch with prefix caching
- **Repo**: vllm-project/vllm#44485
- **Workstream**: Training Hub Integration
- **Impact**: With `extract_hidden_states` + prefix caching enabled, saved safetensors occasionally have mismatched `hidden_states` and `token_ids` lengths — breaks draft training data alignment.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/vllm-project/vllm/issues/44485

### 9. Eagle 2/3 acceptance length regression over time
- **Repo**: vllm-project/vllm#41838
- **Workstream**: Training Hub Integration
- **Impact**: Acceptance length monotonically degrades under sustained Eagle/Eagle3 serving and only recovers on restart — directly affects how you validate trained draft models in production.
- **Effort**: Medium — code + tests
- **Link**: https://github.com/vllm-project/vllm/issues/41838

### 10. Hidden state layer count mismatch in GLM-5.2 DFlash pipeline
- **Repo**: vllm-project/speculators#705
- **Workstream**: Training Hub Integration
- **Impact**: Layer count mismatch between extracted hidden states and DFlash training config — same class of alignment bug you'll hit wiring Eagle3 layer IDs through vLLM extraction into speculators.
- **Effort**: Small — docs/config
- **Link**: https://github.com/vllm-project/speculators/issues/705

## Reviews Needed

No speculative-training PRs found in the 7 core team repos this scan. Active RFC
discussion on vllm#45175 and speculators#584 if you want to engage upstream before
implementing.

## Speculative Training — Dependency Map

```
generate_responses() ← training_hub#86
  └─ vLLM verifier inference
     └─ extract_hidden_states ← vllm#43810 #44485
        └─ speculators training ← #584 #586 #705

eagle3_train() ← training_hub#86
  └─ vLLM speculative decode ← #41838 validation
```

---

### Coverage Summary

| Workstream | Items found | In top 10 |
|---|---|---|
| Training Hub Integration | 14 | 8 |
| Kubeflow SDK | 1 | 1 |
| InstructLab Training | 0 | 0 |
| RLVR/GRPO | 1 | 0 |
| SFT/LoRA Fine-Tuning | 0 | 0 |
| KFP Pipelines | 0 | 0 |
| Training Images | 0 | 0 |
| Guided Examples | 1 | 1 |

Scanned: ~120 speculative-related issues + full team repo sweep across 9 repos (added vllm-project/vllm, vllm-project/speculators)
Generated: July 2, 2026

### Suggested pick order for your sprint:

1. **Own**: training_hub#86 (implementation)
2. **Quick upstream win**: vllm#43810 (fixes corrupted training data today)
3. **Engage RFCs**: speculators#584 + vllm#45175 (align `generate_responses()` with upstream)
4. **Validation**: vllm#41838 (acceptance-rate regression affects how you measure success)

